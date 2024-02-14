# TypeScript - 05: Functions and Decorators

## Introduction to Functions and Decorators in TypeScript

TypeScript enhances JavaScript by adding types to the language. It's not just about adding static types; it's about using these types to write safer and more predictable code. Among TypeScript's features, **functions** and **decorators** play a crucial role in structuring applications efficiently.

## Functions in TypeScript

Functions in TypeScript add powerful typing capabilities, which bring clarity and predictability to function signatures.

### Understanding Function Types

Function types in TypeScript allow developers to specify the types of the inputs and outputs of functions. This ensures that functions are called with the right types of arguments and that they return the right type of result.

```jsx
function greet(name: string): string {
  return "Hello, " + name + "!";
}
```

### Optional and Default Parameters

TypeScript introduces optional parameters that can be omitted in function calls, and default parameters that provide default values.

#### Optional parameter example:

```jsx
function buildAddress(street: string, city: string, country?: string) {
  // Function body
}
```

#### Default parameter example:

```jsx
function buildGreeting(greeting: string = "Hello") {
  // Function body
}
```

### Rest Parameters and Overloads

Rest parameters allow functions to accept an indefinite number of arguments as an array. Overloading allows multiple function signatures for a single function name.

#### Rest parameter example:

```jsx
function sum(...numbers: number[]): number {
  return numbers.reduce((a, b) => a + b, 0);
}
```

#### Function overloading example:

```jsx
function getItems(n: number): number[];
function getItems(s: string): string[];
function getItems(arg: any): any[] {
  // Function body
}
```

## Advanced Function Concepts

### Higher-Order Functions

Higher-order functions are functions that take other functions as arguments or return them. They're a cornerstone of functional programming in TypeScript.

#### Example of a higher-order function:

```jsx
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((n) => n * 2);
```

### Generics with Functions

Generics add the ability to create flexible and reusable functions that can operate on different types.

#### Example of a generic function:

```jsx
function identity<T>(arg: T): T {
  return arg;
}
```

This brief overview touches on the fundamental aspects of functions in TypeScript. The next section will delve into decorators and their application, further expanding TypeScript's capabilities in enhancing and extending behavior in a structured manner.

## Introduction to Decorators in TypeScript

Decorators offer a powerful and expressive way to modify and enhance classes, methods, and properties in TypeScript. They provide a meta-programming syntax for declarative programming.

### Basics of Decorators

Decorators are special kinds of declarations that can be attached to a class declaration, method, accessor, property, or parameter. Decorators use the form `@expression`, where `expression` must evaluate to a function that will be called at runtime with information about the decorated declaration.

#### Decorator Factories

Decorator factories are functions that return a decorator function. They allow customization of the decorator logic.

#### Example of a decorator factory:

```jsx
function color(value: string) {
  return function (target) {
    // decorator logic
  };
}
```

## Practical Use Cases of Decorators

### Class Decorators

Class decorators are applied to the class constructor and can be used to observe, modify, or replace a class definition.

**Example of a class decorator**:

```jsx
function sealed(constructor: Function) {
  Object.seal(constructor);
  Object.seal(constructor.prototype);
}

@sealed
class Greeter {
  greeting: string;
  constructor(message: string) {
    this.greeting = message;
  }
  greet() {
    return "Hello, " + this.greeting;
  }
}
```

### Method and Accessor Decorators

Method decorators are applied to the property descriptor for the method, and accessor decorators are applied to the accessor descriptor.

**Example of a method decorator**:

```jsx
function enumerable(value: boolean) {
  return function (
    target: any,
    propertyKey: string,
    descriptor: PropertyDescriptor
  ) {
    descriptor.enumerable = value;
  };
}

class Greeter {
  greeting: string;
  constructor(message: string) {
    this.greeting = message;
  }

  @enumerable(false)
  greet() {
    return "Hello, " + this.greeting;
  }
}
```

### Property and Parameter Decorators

Property decorators can be used to modify the behavior of a property, and parameter decorators can inject or modify arguments passed to a method.

**Example of a property decorator**:

```jsx
function format(formatString: string) {
  return function (target: any, propertyKey: string) {
    // Property decorator logic
  };
}
```

This overview introduces decorators in TypeScript, showcasing their syntax, types, and practical use cases. Decorators provide a robust set of capabilities for meta-programming, allowing developers to write cleaner, more declarative code by annotating and modifying classes and their members.

## Best Practices and Tips for Functions and Decorators in TypeScript

### Best Practices for Functions

- **Type Annotations**: Always use type annotations for function parameters and return types to leverage TypeScript's type checking.

- **Use Arrow Functions for Shorter Syntax**: When appropriate, use arrow functions for concise syntax, especially for one-liners and functions passed as arguments.

- **Default and Optional Parameters**: Utilize default parameters for functions to simplify logic and avoid undefined checks. Mark parameters that may not always be provided as optional.

```jsx
const greet = (name: string = "Guest"): string => `Hello, ${name}`;
```

- **Rest Parameters for Variadic Functions**: Use rest parameters instead of the arguments object for better type safety and readability.

```jsx
function sum(...numbers: number[]): number {
  return numbers.reduce((acc, current) => acc + current, 0);
}
```

### Best Practices for Decorators

- **Understand Decorator Execution Order**: Remember that decorator factory execution is in the order they appear, but decorator execution is bottom-up.

- **Use Decorators Sparingly**: Decorators are powerful but should be used judiciously to avoid adding unnecessary complexity.

- **Secure Decorator Logic**: Ensure decorators do not unintentionally expose or modify sensitive data.

```jsx
function logMethod(target: any, key: string, descriptor: any) {
  let originalMethod = descriptor.value;
  descriptor.value = function (...args: any[]) {
    console.log(`Called ${key} with args: ${args}`);
    return originalMethod.apply(this, args);
  };
  return descriptor;
}
```

### Common Pitfalls

- **Overusing Decorators**: Applying decorators extensively can make the code harder to understand and maintain.

- **Ignoring TypeScript Compiler Options**: Some decorator features require specific TypeScript compiler options (`experimentalDecorators`, `emitDecoratorMetadata`) to be enabled.

## Conclusion

Functions and decorators in TypeScript significantly enhance the development experience by providing a robust structure for type safety and meta-programming. They allow developers to write more expressive, maintainable, and scalable code. Experimenting with these features opens up new possibilities for application design and can lead to more efficient development workflows.

As you become more comfortable with these advanced features, you'll be well-equipped to tackle even more complex topics in TypeScript. Stay tuned for our next articles, where we'll delve into state management strategies and reactive programming patterns that can further elevate your TypeScript projects.

---

Hi there, I'm Darshan Jitendra Chobarkar, a freelance web developer who's managed to survive the caffeine-fueled world of coding from the comfort of Pune. If you found the article you just read intriguing (or even if you're just here to silently judge my coding style), why not dive deeper into my digital world? Check out my portfolio at [https://darshanwebdev.com/](https://darshanwebdev.com/) – it's where I showcase my projects, minus the late-night bug fixing drama.

For a more 'professional' glimpse of me (yes, I clean up nice in a LinkedIn profile), connect with me at [https://www.linkedin.com/in/dchobarkar/](https://www.linkedin.com/in/dchobarkar/). Or if you're brave enough to see where the coding magic happens (spoiler: lots of Googling), my GitHub is your destination at [https://github.com/dchobarkar](https://github.com/dchobarkar). And, for those who've enjoyed my take on this blog article, there's more where that came from at [https://dchobarkar.github.io/](https://dchobarkar.github.io/). Dive in, leave a comment, or just enjoy the ride – looking forward to hearing from you!
