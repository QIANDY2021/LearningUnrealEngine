2020-12-26_20:22:38

# Niagara events

(For 4.25, events only work for CPU particles, not GPU.)

Requires Persistent IDs must be enabled in the Emitter Properties of emitters that use events.
Not sure if this is a requirement on both event triggers and event handlers.

Events are a way to communicate between emitters in a system.
One emitter generates some data and emitters listens for and reacts to that data.
We say that the listening emitter has an Event Handler for an Event.
Both Events and Event Handlers are Modules.
An event trigger can send arbitrary payload, i.e., an attribute container, to the event handlers.
An event handler can run on a particular particle  (by ID) or on every particle.
An event handler can spawn new particles and run on those.
An event handler has it's own execution stack. (Module stack?)
Splitting the logic up between the main emitter stack and a set of event stacks help with organization.
There are built-in events for collision, death, and location.
Events can be any kind of data, packed into a payload (such as a struct) and sent. Then anything else can listen for that event and take action.
An event can be run directly on a particle using Particle.ID, on every particle in a system, or spawn a collection of particles on run on those.

### Location Events

Location events are enabled by adding a Generate Location Event module to the Particle Update group in an emitter.
Particles spawned by that emitter will continuously generate location events.
The location event has a payload that contains a bunch of attributes or the particle.
Position, Velocity, and normalized age, for example.
The payload attributes can be linked to any attribute.

### Death events

Generated by placing a Generate Death Event module in the Particle Update group.
The event is triggered at the end of each particle's lifetime.

### Collision events

Generated by placing a Generate Collision Event module in the Particle Update group.
The event is triggered when a particle collides with an Actor, such as a Static or Skeletal Mesh.
Collision events can only be generated if collision data is available.
Collision data is generated by the Collision module.
The Collision module must be above the Generate Collision Event module in the module stack.

### Event Handlers

Event Handlers are added to Emitters.
An Event Handler consists of an Event Handler Properties and a Receive Event module.
The Event Handler Properties decide what event source the handler should listen to.
You can bind tho any event in any Emitter in the System.
The Receive Event module is the code that is executed when the event is triggered by the source.
You need such a pair for each event the emitter should listen to.
The Source property of the Event Handler Properties is one of the Events that exists in the current System.
An Event Handler can spawn new particles.
The Event Handler can operate either on all particles, a set of particles based on the particle's ID, or on particles spawned by the Event Handler.


(
I do not understand how to add new types of events, or even how to create new Modules that respond to existing events.
The `ReceiveLocationEvent` Module contains a `LocationEvent Read` node that I'm not able to create in my own script. It doesn't show up in the Node Graph context menu.
I can copy-paste the node from the Receive Location Event module to my module.
Cannot use that module though:
```
Missing definition string.

The Vector VM compile failed.  Errors:
/Engine/Generated/NiagaraEmitterInstance.ush(329): error: syntax error, unexpected '.', expecting ',' or ';'

 NE_Follow, Particle Event Script, 
```
This seems to be because I have two modules with the `ReceiveLocationEvent` node in the same Event Handler.
If I disable their `ReceiveLocationEvent` module and re-implement the parts of it that I need in my module that it works.
)

### Custom events

A custom event is generated from a module by adding an `Add <TYPE> Event Write` node, where `<TYPE>` is the type of the payload to send.
The module must be in the Particle Update module group, it seems.
Not sure, but events generated in Emitter Updates aren't picked up by the Source drop-down list in Event Handler Properties.


A custom event is received in a module by adding an `Add <TYPE> Event Read` node, where `<TYPE>` is the type of the payload to receive.
The module must be added to 


[How to Add Enums And Structs To Niagara @ niagara-vfx.herokuapp.com](https://niagara-vfx.herokuapp.com/how-to-add-enums-and-structs-to-niagara/#)