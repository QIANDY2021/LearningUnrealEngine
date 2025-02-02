2020-08-26_08:28:16

# Build systems

A "build system" in this text is a compiler front-end / runner along with project-specific files that describe how the different parts of the project should be built.
Examples of build systems are Visual Studio Solutions, Makefiles, and in some sense also CMake projects.
Unreal Engine can generate project files for various build systems to make building C++ projects easier.
So that one doesn't need to manually call `Build.sh` or Unreal Build Tool.

Project files for several build systems can be generated for a project.
On Windows people seem to generally use Visual Studio.
On Linux there is a bit more variability, but common options are Makefiles and CMake.
Project files are generated by running the `GenerateProjectFiles.sh` script.
It's located in the root of the Unreal Engine working copy.
Pass it the path to the `.uproject` file and parameters controlling how and for what to generate project files.
Example:
```
./GenerateProjectFiles.sh <PATH>/<PROJECT>.uproject -game -Makefile -CMakefile
```
The above will generate a `Makefile` and a `CMakeLists.txt` in `<PATH>`,

Unreal Engine 4.25 introduced a warning when passing project type parameters on the command line.
Recommends setting these in `BuildConfiguration.xml` instead.
On Linux this file is at `~/.config/Unreal Engine/UnrealBuildTool/`.
```
<?xml version="1.0" encoding="utf-8" ?>
<Configuration xmlns="https://www.unrealengine.com/BuildConfiguration">
  <ProjectFileGenerator>
    <Format>Make</Format>
    <Format>CMake</Format>
  </ProjectFileGenerator>
</Configuration>
```
This doesn't work.

## Fixing red squiggles

A bug introduced in Unreal Engine 4.25 broke IDE, e.g. CLion, integration. Someone has prepared engine source fixes patch patches for it:
- 4.25: https://gist.github.com/ericwomer/142650e65473087073f30e5fb97fd6e8
- 4.26: https://gist.github.com/jerobarraco/92839db6e6305fb04a04bab415ec8ae4
- 4.27: https://gist.github.com/aknarts/7d7367fa5e5e54fe30be3bd6b67cf59d


In short, the problem is in `Engine/Source/Programs/UnrealBuildTool/Configuration/UEBuildModuleCPP.cs` and the cause is that `CompileEnvironment.Definitions` is cleared too early.

Symptoms are Unreal Engine macros not being recognized, missing class definitions, undeclared definitions.

## Listing compiler commands

Unreal Build Tool is supposed to be able to generate a list of native compiler invocations with `UnrealBuildTool.exe -mode=GenerateClangDatabase`, but I have not been able to get it to work.


## Compiler flags

One can not make changes to the generated project files to add or remove build flags.
That's all handled by Unreal Build Tool and it doesn't take the settings in the build files into account.
There is some control over build flags through the per-module `.Build.cs` files.
To change it globaly you must edit the engine code for Unreal Build Tool.
- Linux platform: `Engine/Source/Programs/UnrealBuildTool/Platform/Linux/LinuxToolChain.cs`.
- CMake build environment: `Engine/Source/Programs/UnrealBuildTool/ProjectFiles/CMake`.

Changes to `.cs` files part of Unreal Build Tool will be detected and the tool rebuilt when running `GenerateProjectFiles.sh`.


[[20200827122445]] Third-party libraries