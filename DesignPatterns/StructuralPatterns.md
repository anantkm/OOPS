# Structural Design Patterns

## Adapter

### Explanation
Allows objects with incompatible interfaces to collaborate by wrapping its own interface around that of an already existing class.

### Example
Enabling data from a legacy system to be used by newer systems through a compatibility interface.

### Pros
- Allows for collaboration between classes with incompatible interfaces.

### Cons
- Increases overall complexity of the code by introducing additional layers of abstraction.

## Bridge

### Explanation
Decouples an abstraction from its implementation so that the two can vary independently.

### Example
A program's GUI interface could change independently from the underlying operating system functionality it uses.

### Pros
- Enhances flexibility in varying both the interface and implementation.

### Cons
- Increases complexity by splitting functionality into separate classes.

## Composite

### Explanation
Allows composing objects into tree structures to represent part-whole hierarchies, treating individual objects and compositions uniformly.

### Example
Graphic objects in a drawing application which can be composed into complex structures like maps or diagrams.

### Pros
- Simplifies the client code that uses the objects.

### Cons
- Can make the design overly general, complicating the object structure.

## Decorator

### Explanation
Adds new functionality to objects dynamically by placing them inside special wrapper objects that contain the behaviors.

### Example
Adding encryption to a stream of data being read/written from/to a file dynamically.

### Pros
- More flexible than static inheritance for extending functionality.

### Cons
- Can result in a large number of small classes which can be confusing.

## Facade

### Explanation
Provides a simplified interface to a complex subsystem, reducing the overall complexity of the system.

### Example
An easy to use interface for file operations that masks the complexity of the underlying file system.

### Pros
- Simplifies the interactions with complex systems.

### Cons
- Can become a bottleneck if not implemented efficiently.

## Flyweight

### Explanation
Minimizes memory use by sharing as much data as possible with other similar objects.

### Example
Sharing the character data in a document editor to reduce memory usage when the same character appears multiple times.

### Pros
- Reduces the amount of memory and storage needed when large numbers of objects are used.

### Cons
- Complexity in managing shared objects.

## Proxy

### Explanation
Provides a surrogate or placeholder for another object to control access to it.

### Example
A remote proxy controlling access to an object located in a different address space.

### Pros
- Can control the creation and access of costly objects.

### Cons
- Can introduce a delay in the access to an object due to the initial loading process.
