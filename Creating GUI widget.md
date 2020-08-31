2020-07-04_19:53:32

# Creating GUI widget

A widget is an asset, so it exists within the Content Browser.
Right-click > User Interface > Widget Blueprint.
Double-clicking a Widget Blueprint opens the Widget Editor.
It contains a Design Board, in the Designer tab, and a Node Graph, in the Graph tab.
Switch tabs in the upper right corner.
GUI Widgets are used both for in-game HUD elements and for full-screen menus.
Variables held by Actors can be displayed in the GUI Widget.

Widgets are added by dragging them from the Palette panel into either the Design Board or the Hierarchy panel.
Widgets are renamed in the top of the widget's Details panel.

Many widets display data.
Such widgets can have a binding.
A bindings is a function that fetch and format that data.
Create a binding for a data field in a Widget's Detals panel by clicking Bind > Create New Binding next to the data field in the Details panel.
A new function is created.
The functions can be renamed.

Root Widgets must be instantiated before they are seen.
This can be done from BeginPlay, for example in a PlayerController of the Level Blueprint.
An instance if create with Create Widget.
Select your Widget class from the drop down.
The created Widget is often promoted to a variable.
Next add it to the viewport.



[[2020-08-10_20:32:35]] UMG
[[2020-08-10_20:34:24]] Widget Blueprint