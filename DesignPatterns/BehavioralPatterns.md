# Behavioral Design Patterns

## Chain of Responsibility

### Explanation
Creates a chain of receiver objects for a request. This pattern allows a request to be passed along the chain until it is handled.

### Example
A help system where a request can go from a simple FAQ handler up to higher levels of support if the initial handlers cannot serve the request.

### Pros
- Decouples sender and receiver of a request based on handling capability.

### Cons
- It can lead to a long processing time as the request passes through the chain.

## Command

### Explanation
Encapsulates a command request as an object, thereby allowing users to parameterize clients with queues, requests, and operations.

### Example
GUI buttons and menu items in an application that perform actions like opening files or exiting the application.

### Pros
- Allows the separation of concerns between object requesting the operation and objects knowing how to perform it.

### Cons
- Can lead to an increase in the number of classes for each individual command.

## Iterator

### Explanation
Provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation.

### Example
Navigating through a collection of objects in a uniform manner, regardless of the underlying data structure.

### Pros
- Provides a standard way to cycle through a collection without exposing its implementation.

### Cons
- If not careful, iterators can be inefficient in terms of memory or complexity.

## Mediator

### Explanation
Defines an object that encapsulates how a set of objects interact. Mediator promotes loose coupling by keeping objects from referring to each other explicitly.

### Example
A chat room acting as a mediator between different users.

### Pros
- Reduces the dependency between components by centralizing complex communications and control logic.

### Cons
- The mediator can become a god object coupled with all classes it governs.

## Memento

### Explanation
Provides the ability to restore an object to its previous state (undo via rollback).

### Example
The undo feature in a text editor that restores text to its previous state after changes.

### Pros
- Enables recovery from mistakes or changes in state without exposing details of implementation.

### Cons
- Saving and restoring states can consume significant resources.

## Observer

### Explanation
Defines a dependency relationship between objects so that whenever an object changes its state, all its dependents are notified and updated automatically.

### Example
A weather station broadcasts updates to various display elements as the weather changes.

### Pros
- Promotes layer independence. Objects can interact without forming tight coupling.

### Cons
- Can lead to unintended performance impacts if the number of observers or the frequency of updates is high.

## State

### Explanation
Allows an object to alter its behavior when its internal state changes. The object will appear to change its class.

### Example
A document being in different states such as Draft, Review, and Published with different behaviors in each state.

### Pros
- Encapsulates varying behavior for the same routine based on state.

### Cons
- Increasing number of states can increase the complexity of maintenance.

## Strategy

### Explanation
Defines a family of algorithms, encapsulates each one, and makes them interchangeable. Strategy lets the algorithm vary independently from clients that use it.

### Example
Different sorting algorithms used within an application, selectable based on the size or type of data.

### Pros
- Provides flexibility to switch algorithms at runtime.

### Cons
- Clients must be aware of the differences between strategies to select the appropriate one.

## Template Method

### Explanation
Defines the skeleton of an algorithm in the method, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.

### Example
A game with a fixed set of steps to play (initialize game, start play, end play) where the specifics can be defined in subclasses.

### Pros
- Standardizes the overarching procedure, allowing subclasses to implement specific behaviors.

### Cons
- Can lead to a less flexible design since changing the template method can impact subclasses.

## Visitor

### Explanation
Lets you define a new operation without changing the classes of the elements on which it operates.

### Example
Adding new operations to be performed on the elements of a complex object structure, like a document object model.

### Pros
- Facilitates adding operations to object structures without modifying the structures.

### Cons
- Can lead to violations of encapsulation as the visitor needs to access internals of the elements.
