---
title: Engine Overview
permalink: /docs/engine-overview/
---

### Engine Intent and Usage

The intent of this engine is to provide game developers with a powerful yet flexible game creation platform upon which features can easily be added and customized. Typical usage will involve accessing and supplying data to different modules within the engine, such that the game developer doesn’t need to worry about the underlying framework. Instead, developers can use the intuitive abstractions provided by the engine, allowing them to spend more time focussing on actually making games rather than worrying about how to fit everything together.

### Current Engine Functionality Goals

#### Core Functionality
- Things like running the main engine loop, multithreading, concurrent communication (events, queues, etc.)
- Entirely specific to the engine and is what drives everything else

#### Graphics Functionality
- In short, draws stuff to the screen. Doing so will require different layers of a wide range of abstractions, from textures all the way up to scene state
- Almost entirely specific to the engine, although allowing of external customization within the conditions defined by the graphics system (ie. the game can provide model data to be rendered)

#### Physics Functionality
- Simulates the physical interactions between objects in a virtual world
- Implementation will be decided upon at a later date, however, like the engine’s graphics functionality, nearly all of the physics functionality should be specific to and defined by the engine

#### Input Functionality
- Collects raw input and distributes it to all relevant game entities
- Should be largely accessible and modifiable by the game logic, either through defining input response in components supplied to the engine, or directly receiving and reading input events from the system itself


### Current Engine Design Philosophy

#### Modular Structure
- Each module should encompass its own state, behaviour, and functionality, and should be able to be used in isolation of any other modules; virtually self contained
- Each module will need to implement and define its own runtime lifecycle, accessible by the engine core. Such an interface should include methods to start, update, and shutdown the module. Additionally, modules can build upon these base methods
- A good guide to follow would be in organizing modules by systemic dependencies

#### Functional Abstractions
- Modules must provide a balance between function and abstraction. While the low-level APIs and classes should be abstracted away from the game logic, the remaining interface between game and engine should still provide ample functionality for working with modules

#### Flexibility
- Each module, and the engine as a whole, should provide a certain degree of behavioural flexibility to the game logic. This means that the game logic should be able to define and supply its own implementations of behaviour and function to applicable modules

#### Separation Between Game and Engine
- In order to correctly design the engine architecture, a clear distinction between systems specific to the engine and systems specific to game logic must be drawn


### Useful Design Patterns

#### Command Pattern
- Used to decouple method calls and allow for function interchangeability
- Lambdas are used to decouple method calls
- Intrasystem
- Allows for more direct, explicit communication and should therefore be used between more tightly related objects

#### Observer Pattern + Event Queue for Multithreading
- Used specifically for observing events of interest, and then deciding how to act on that
- Relies on Pub/Sub method of communication
- Whereas the Command pattern decouples method calls (intrasystem), the Observer Pattern decouples systems entirely (intersystem)
  - For example, the physics system shouldn’t have any knowledge of the audio system, and therefore shouldn’t be responsible for calling .playSound() when there is a collision. Instead, it publishes a collision event, which is routed to registered observers, such as the audio system, who choose how to respond
- Its use should be reserved for objects/systems with little to no relation and can function without the other objects/systems. Otherwise, the Command Pattern would be a better fit
- Inactive observers must be unregistered
- Observer methods that handle notification events should avoid sending events inside the notification-handling code in order to prevent feedback loops

#### Singleton Pattern
- A Singleton is a static, globally accessible object, and is bad because it increases coupling between every class that accesses it, especially classes that are unrelated. It also makes multithreading more of a challenge
- Instead of having multiple global objects, have a single globally accessible object that provides access to instances of other objects
- Honestly though, the real problem with globals comes down to when you have a large team of people working on a project, where people have access to classes that they shouldn’t be touching without knowing the inner workings of that global class or other classes that access it

#### State Pattern + Double Buffer
- States are essentially structured changes to an object’s behaviour
- A State Interface outlines the behavioural structure of a type of object
- State Classes implement the State Interface, and define their own behaviours based on the interface’s structure
- State Classes/Objects are responsible for providing the parent object with transitions to other states
  - For example, a LoadingScreen state object will return an instance of the GameWorld state object to the Game parent object once the game has finished loading
- Transitions are composed of two parts: entry and exit actions
  - Exit actions are simply having a state object return an instance of another state object when an internal event calls for a state change, as well as any other operations that need to be performed when a state exits
  - Entry actions are performed by every state through a method call when said state is made active by the parent object
- It’s important to remember that game logic implementations of state such as “GameScreen” or “CharacterAnimation” are to be kept out of the engine (see: **Separation Between Game and Engine**)

#### Component Pattern
- One universal game entity class that holds all components. Entities provide references to specific objects that inherit general component interfaces. For example, the AIPhysicsComponent implements the PhysicsComponent interface, with the GameEntity class holding a reference to any implementation of the PhysicsComponent interface
- Component interfaces are created to access and use the functionality of existing systems, such as the Renderer
- While the engine provides all the component interfaces, it is up to the game logic to create its own implementations of each component type. Note that the game logic should not be extending the entity class, since behaviour and function are already defined through an entity’s components
- The game logic is also responsible for adding component objects to the game entity
- Inter-component communication is facilitated through public variables in the GameEnitity class - useful for state that is shared amongst the majority of components (position, rotation, velocity), references to other components, or through the use of the command pattern
- The update pattern is utilized for each component. Rather than updating the entity itself, update methods for every component in an entity are called


### Structure

- → Engine
- → EngineConfig(?)
- → Engine Library
  - ↳ GameEntity
  - ↳ Scene
  - ↳ IModule
  - ↳ IState
  - ↳ ICommand
  - ↳ EngineState
- → Modules
  - ↳ Core
     - ↳ Module
     - ↳ EngineLoop
     - ↳ MessageQueue
     - ↳ ModuleCSM
  - ↳ Graphics
    - ↳ Module
    - ↳ Renderer
    - ↳ RenderState
    - ↳ IRenderComponent
  - ↳ Input
    - ↳ Module
    - ↳ InputDispatcher
    - ↳ InputState
    - ↳ IInputComponent
  - ↳ ModelLibrary
    - ↳ Module
    - ↳ ModelLoader
    - ↳ Sprite
    - ↳ PointLight
  - ↳ Tools
    - ↳ Logger
- → Framework
  - ↳ OpenGL
    - ↳ Window
    - ↳ ShaderProgram
    - ↳ Camera
    - ↳ Texture
    - ↳ Mesh
  - ↳ IO
    - ↳ KbMInput
    - ↳ FileIO
    - ↳ SQLIO


### In Practice

#### Engine
- Game logic entry point

#### Engine Library
- Allows for user-defined configuration of the engine and provides abstractions for modules

#### Modules
- Interact directly with the engine, defining its function and behaviour as a whole

  **Core**
  - Contains all classes required for core functionality of the engine. This includes things like the engine loop, a messaging queue, concurrent module state machine, etc.

  **Graphics**
  - Contains all classes required for graphical functionality of the engine, including rendering and renderable object management. It holds things such as the render state, components, and the OpenGL renderer itself

  **Input**
  - Contains classes responsible for gathering and dispatching input to interested parties. Here, the game logic is free to register for input events by specifying a command that executes an action (method call) when prompted by the event of interest. These commands are mapped to their respective input event and generated when needed

  **ModelLibrary**
  - Includes all graphical models such as sprites and point lights, as well as a model loader for the game logic to use

  **Tools**
  - Tools such as the logger

#### Framework
- Provides low level access to external libraries. There shouldn’t be any dependencies between classes

  **OpenGL**
  - Graphics wrapper needed to abstract the OpenGL API’s state-machine nature into a friendlier, object oriented design for the rest of the engine to use

  **IO**
  - Contains all classes that deal with raw input and output, and are responsible for translating that input data into objects that can be used by higher level systems. This includes keyboard and mouse/controller IO, file IO, and database IO
