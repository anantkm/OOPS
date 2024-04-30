### Creational Patterns

1. **Factory Method**
   - **Explanation**: This pattern provides an interface for creating objects in a superclass but allows subclasses to alter the type of objects that will be created.
   - **Example**: A document editor can use a factory method to create different types of documents (like TextDocument or Spreadsheet) based on user input.
   - **Pros**: Enhances object creation flexibility and encapsulation.
   - **Cons**: Can introduce a lot of subclasses, increasing complexity.

2. **Abstract Factory**
   - **Explanation**: Offers an interface for creating families of related or dependent objects without specifying their concrete classes.
   - **Example**: A furniture factory that creates furniture sets (like VictorianChair, VictorianSofa) where each set items share a common theme.
   - **Pros**: Isolates concrete classes and promotes consistency among products.
   - **Cons**: Hard to support new kinds of products without altering the interface.

3. **Builder**
   - **Explanation**: Separates the construction of a complex object from its representation so that the same construction process can create different representations.
   - **Example**: A meal builder assembling a meal, with choices over steps like selecting a drink, main item, and dessert.
   - **Pros**: Provides control over the construction process and allows for step-by-step construction.
   - **Cons**: Can complicate the code due to the need for multiple builder classes.

4. **Prototype**
   - **Explanation**: Enables copying existing objects without making your code dependent on their classes.
   - **Example**: Cloning a graphic object like a circle that has been customized with size and color, instead of creating a new one from scratch.
   - **Pros**: Simplifies object creation by cloning, especially useful when object creation is expensive.
   - **Cons**: Cloning complex objects with circular references can be tricky.
