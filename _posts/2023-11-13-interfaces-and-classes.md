# TypeScript - 03: Interfaces and Classes

## Introduction to TypeScript: Elevating JavaScript Development

TypeScript, a brainchild of Microsoft, has revolutionized the landscape of web development by introducing static typing to JavaScript. At its core, TypeScript extends JavaScript, adding type safety and enabling developers to catch errors early in the development process, thereby significantly enhancing code quality and readability. Interfaces and classes, two cornerstone features of TypeScript, play a pivotal role in structuring complex applications and promoting code reusability.

## Understanding Interfaces in TypeScript

### The Essence of Interfaces in TypeScript

Interfaces in TypeScript are powerful constructs that describe the shape of objects, enforcing a contract on the properties and functions that an object must have. Unlike classes, interfaces are purely a TypeScript compile-time construct and do not exist at runtime.

### Basic Interface Examples

Consider an interface `Person` that requires an object to have a name and age:

```jsx
interface Person {
  name: string;
  age: number;
}
```

To apply this interface to an object, simply ensure the object meets the structure defined by the interface:

```jsx
let employee: Person = {
  name: "John Doe",
  age: 30,
};
```

Interfaces can include optional properties and readonly modifiers, enhancing flexibility and immutability:

```jsx
interface Vehicle {
  readonly make: string;
  model: string;
  year?: number; // Optional property
}
```

### Extending Interfaces

TypeScript allows interfaces to extend one another, promoting reusability and extending object shapes:

```jsx
interface Shape {
  color: string;
}

interface Square extends Shape {
  sideLength: number;
}
```

## Advanced Interface Concepts

### Function Types in Interfaces

Interfaces can also describe function types, allowing for a formal definition of the function's signature:

```jsx
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc = function (source: string, subString: string) {
  return source.search(subString) > -1;
};
```

### Indexable Types

For arrays or objects with specific types, indexable types come into play, allowing you to specify the type of indexed access:

```jsx
interface StringArray {
  [index: number]: string;
}

let myArray: StringArray = ["Bob", "Fred"];
```

### Implementing an Interface with a Class

TypeScript classes can implement interfaces, ensuring instances of the class adhere to the structure defined by the interface:

```jsx
class Car implements Vehicle {
  readonly make = "Ford";
  model = "Fiesta";
  constructor(public year: number) {}
}

let myCar = new Car(2020);
```

These examples barely scratch the surface of TypeScript's capabilities. Interfaces in TypeScript not only ensure your objects adhere to specific contracts but also significantly improve your development experience with rich IntelliSense and compile-time checks. By embracing interfaces, you're not just writing code; you're crafting a blueprint for your application's data structures, leading to more predictable and reliable codebases.

## Introduction to Classes: The Backbone of Object-Oriented TypeScript

### The Concept of Classes in TypeScript

In TypeScript, classes serve as blueprints for creating objects, encapsulating data for the object and methods to operate on that data. A class typically includes properties (to hold values) and methods (to perform actions).

```jsx
class Person {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  greet() {
    console.log(`Hello, my name is ${this.name}!`);
  }
}
```

The `constructor` method is a special function that gets called when a new instance of the class is created, utilizing the `this` keyword to access class members.

## Access Modifiers: Controlling Visibility

### Encapsulating Class Members

TypeScript introduces access modifiers `public`, `private`, and `protected` to control access to class members, enhancing encapsulation and security.

- **Public**: Members are accessible from any location. This is the default if no access modifier is specified.

- **Private**: Members are only accessible within the class they are declared in.

- **Protected**: Members are accessible within the class they are declared in and by instances of derived classes.

```jsx
class Animal {
  public name: string;
  private age: number;
  protected type: string;

  constructor(name: string, age: number, type: string) {
    this.name = name;
    this.age = age;
    this.type = type;
  }
}
```

## Classes and Inheritance: Extending Functionality

### Building on Existing Classes

TypeScript allows classes to inherit from other classes, a fundamental aspect of object-oriented programming. Inheritance enables new classes to absorb properties and methods of an existing class.

```jsx
class Dog extends Animal {
  constructor(name: string, age: number) {
    super(name, age, "dog");
  }

  bark() {
    console.log("Woof!");
  }
}
```

In the `Dog` class, `super` is called to execute the parent class's constructor, and a new method, `bark`, is introduced. The protected `modifier` allows the `type` property from the `Animal` class to be accessed within the `Dog` class, showcasing polymorphism and encapsulation.

TypeScript’s class system is a powerful abstraction, providing clear syntax for object creation, encapsulation through access modifiers, and code reusability via inheritance. By leveraging these concepts, developers can write more organized, robust, and easily maintainable code. The next part of this series will delve into TypeScript’s advanced types and their application in real-world scenarios, further solidifying the foundation for building enterprise-level applications.

## Interfaces vs. Classes: A Strategic Choice

### Making the Right Choice

In TypeScript, both interfaces and classes play significant roles, but understanding when to use one over the other is crucial for efficient coding practices.

- **Interfaces** are primarily used to define the shape of an object or the contract within your code. They are ideal for when you need to define a type shape or a contract between different parts of your application but do not need to implement the functionality directly.

- **Classes**, on the other hand, are used when you need to create instances or implement functionality that can be reused across the application.

```jsx
interface IAnimal {
  name: string;
  makeSound(): void;
}

class Dog implements IAnimal {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  makeSound() {
    console.log(`${this.name} says Woof!`);
  }
}
```

The decision between an interface and a class often boils down to whether you need to instantiate the type or simply describe a structure.

## Best Practices: Maximizing the Power of TypeScript

### Crafting Quality TypeScript Code

To harness the full potential of interfaces and classes in TypeScript, consider the following best practices:

- Prefer interfaces when defining shapes or contracts for your objects, especially when dealing with the external interface of a module.

- Use classes when you need to create concrete instances or when you need to utilize features like inheritance and access modifiers to encapsulate your code.

- Leverage TypeScript’s access modifiers within classes to protect and encapsulate your class members, ensuring that your objects are used as intended.

```jsx
class Cat implements IAnimal {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  makeSound() {
    console.log(`${this.name} says Meow!`);
  }
}
```

## Conclusion: Building Foundations for Advanced Applications

Interfaces and classes are foundational to developing applications in TypeScript, offering a blend of robust typing and object-oriented programming paradigms. By understanding when and how to use these constructs, developers can create more structured, scalable, and maintainable applications.

As you become more comfortable with interfaces and classes, you're encouraged to explore these concepts in your projects, discovering firsthand how they can improve your development process. The journey into TypeScript continues, with future articles delving into advanced features and design patterns that unlock even more possibilities for your applications.

Stay tuned for our next discussion in the TypeScript series, where we'll explore advanced features and patterns, further enriching your TypeScript development toolkit.

---

Hi there, I'm Darshan Jitendra Chobarkar, a freelance web developer who's managed to survive the caffeine-fueled world of coding from the comfort of Pune. If you found the article you just read intriguing (or even if you're just here to silently judge my coding style), why not dive deeper into my digital world? Check out my portfolio at [https://darshanwebdev.com/](https://darshanwebdev.com/) – it's where I showcase my projects, minus the late-night bug fixing drama.

For a more 'professional' glimpse of me (yes, I clean up nice in a LinkedIn profile), connect with me at [https://www.linkedin.com/in/dchobarkar/](https://www.linkedin.com/in/dchobarkar/). Or if you're brave enough to see where the coding magic happens (spoiler: lots of Googling), my GitHub is your destination at [https://github.com/dchobarkar](https://github.com/dchobarkar). And, for those who've enjoyed my take on this blog article, there's more where that came from at [https://dchobarkar.github.io/](https://dchobarkar.github.io/). Dive in, leave a comment, or just enjoy the ride – looking forward to hearing from you!
