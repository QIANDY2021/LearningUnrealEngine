2020-08-06_18:48:41

# Component Visualizer

A Component Visualizer is something that knows how to render a specific type of Component.
Often used for things that have no inherent visual representation, such as an audio source.
Create a new class inheriting from `FComponentVisualizer`.
Register the visualizer in a module's `StartModule` (or whatever it's name is).
```c++
TSharedPtr<FHT_SeatExitVisualizer> SeatVisualizer = MakeShareable(
    new FHT_SeatExitVisualizer());
GUnrealEd->RegisterComponentVisualizer(
    UHT_SeatExitComponent::StaticClass()->GetFName(),
    SeatVisualizer);
```