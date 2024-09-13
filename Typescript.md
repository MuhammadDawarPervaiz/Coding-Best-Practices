# TypeScript Coding Best Practices

Welcome to the TypeScript Coding Best Practices guide! This document provides essential guidelines for writing clean, maintainable, and efficient TypeScript code.

## Table of Contents

- [Typescript best practices](Typescript.md)
- [General Guidelines](#general-guidelines)
- [Code Style](#code-style)
- [Naming Conventions](#naming-conventions)
- [Type Annotations](#type-annotations)
- [Functions and Methods](#functions-and-methods)
- [Error Handling](#error-handling)
- [Performance](#performance)
- [Testing](#testing)
- [Security](#security)
- [Documentation](#documentation)
- [Resources](#resources)

## General Guidelines

1. **Use TypeScript Strict Mode**: Enable strict mode in `tsconfig.json` to leverage TypeScript's type-checking capabilities and avoid common pitfalls.

   ```json
   {
     "compilerOptions": {
       "strict": true
     }
   }
   ```

2. **Leverage Type Inference**: TypeScript’s type inference is powerful. Use it to your advantage, but always provide explicit types when it improves code clarity.

3. **Avoid Using `any`**: Refrain from using `any` as it bypasses TypeScript’s type-checking. Use specific types or `unknown` where necessary.

4. **Prefer Interfaces over Types for Object Shapes**: Use `interface` for defining object shapes and `type` for more complex type compositions.

   ```typescript
   // Good
   interface User {
       name: string;
       age: number;
   }

   // Good
   type Admin = User & { role: string; };
   ```

## Code Style

1. **Indentation**: Use 2 spaces for indentation to maintain readability and consistency.

2. **Semicolons**: Always use semicolons to terminate statements to avoid potential issues with automatic semicolon insertion.

3. **Line Length**: Limit lines to 80 characters to improve readability and maintainability.

4. **Spaces**: Use spaces around operators and after commas for better readability.

   ```typescript
   // Good
   const sum = a + b;
   let items = [1, 2, 3];

   // Bad
   const sum=a+b;
   let items=[1,2,3];
   ```

5. **Braces**: Always use braces `{}` for control statements (if, for, while) even for single statements.

   ```typescript
   // Good
   if (condition) {
       doSomething();
   }

   // Bad
   if (condition)
       doSomething();
   ```

## Naming Conventions

1. **Variables and Functions**: Use `camelCase` for variable and function names.

   ```typescript
   let userName = 'JohnDoe';
   function calculateTotal(price: number, tax: number): number {
       return price + tax;
   }
   ```

2. **Constants**: Use `UPPER_CASE` for constants.

   ```typescript
   const MAX_ATTEMPTS = 5;
   ```

3. **Classes**: Use `PascalCase` for class names.

   ```typescript
   class UserProfile {
       constructor(public name: string) {}
   }
   ```

4. **Interfaces**: Use `PascalCase` for interface names.

   ```typescript
   interface User {
       name: string;
       age: number;
   }
   ```

5. **Files**: Use `kebab-case` for filenames.

   ```plaintext
   user-profile.ts
   calculate-total.ts
   ```

## Type Annotations

1. **Explicit Types for Function Parameters and Return Values**: Always specify types for function parameters and return values to ensure code clarity and correctness.

   ```typescript
   function greet(name: string): string {
       return `Hello, ${name}!`;
   }
   ```

2. **Use Union and Intersection Types Wisely**: Utilize union types (`type1 | type2`) and intersection types (`type1 & type2`) to represent flexible and composite types.

   ```typescript
   type Status = 'pending' | 'completed' | 'failed';

   interface User {
       name: string;
       age: number;
   }

   type Admin = User & { role: string; };
   ```

3. **Prefer `unknown` over `any`**: Use `unknown` to force type checks before performing operations, which is safer than `any`.

   ```typescript
   function handleInput(input: unknown) {
       if (typeof input === 'string') {
           console.log(input.toUpperCase());
       }
   }
   ```

## Functions and Methods

1. **Small Functions**: Keep functions small and focused on a single task to enhance readability and maintainability.

2. **Use Arrow Functions for Anonymous Functions**: Prefer arrow functions for anonymous functions and callbacks to avoid issues with `this` binding.

   ```typescript
   // Good
   const add = (a: number, b: number): number => a + b;

   // Bad
   function add(a: number, b: number): number {
       return a + b;
   }
   ```

3. **Default Parameters**: Use default parameters to handle optional arguments.

   ```typescript
   function greet(name: string = 'Guest'): string {
       return `Hello, ${name}!`;
   }
   ```

4. **Avoid Overloading When Possible**: Use function overloads only when necessary, and prefer type unions or optional parameters to achieve similar results with simpler code.

   ```typescript
   // Use function overloads only if absolutely necessary
   function print(value: string): void;
   function print(value: number): void;
   function print(value: any): void {
       console.log(value);
   }
   ```

## Error Handling

1. **Use Try-Catch**: Employ `try-catch` blocks to handle exceptions and provide meaningful error messages.

   ```typescript
   try {
       // Code that may throw an error
   } catch (error) {
       console.error('An error occurred:', error);
   }
   ```

2. **Avoid Swallowing Errors**: Always handle errors properly and avoid suppressing them without action.

   ```typescript
   // Good
   if (!response.ok) {
       throw new Error('Network response was not ok');
   }

   // Bad
   if (!response.ok) {
       // Swallowed error
   }
   ```

## Performance

1. **Minimize Unnecessary Type Assertions**: Avoid excessive type assertions as they can lead to runtime errors. Use TypeScript's type inference whenever possible.

2. **Optimize Array and Object Operations**: Use efficient algorithms and data structures to handle large datasets or frequent operations.

   ```typescript
   // Good
   const items = [1, 2, 3, 4, 5];
   const doubled = items.map(item => item * 2);

   // Bad
   const items = [1, 2, 3, 4, 5];
   const doubled = [];
   for (let i = 0; i < items.length; i++) {
       doubled.push(items[i] * 2);
   }
   ```

## Testing

1. **Write Unit Tests**: Ensure your code is covered by unit tests to catch bugs early and verify functionality.

2. **Use a Testing Framework**: Utilize testing frameworks like Jest, Mocha, or Jasmine for consistent testing practices.

   ```typescript
   // Example using Jest
   test('adds 1 + 2 to equal 3', () => {
       expect(add(1, 2)).toBe(3);
   });
   ```

3. **Mock Dependencies**: Use mocking libraries to isolate units of code and test them independently.

## Security

1. **Avoid `eval()`**: Do not use `eval()` to prevent security vulnerabilities such as code injection.

2. **Sanitize User Input**: Always validate and sanitize user input to protect against injection attacks like XSS (Cross-Site Scripting).

3. **Use HTTPS**: Ensure your web applications use HTTPS to encrypt data transmitted between the client and server.

## Documentation

1. **Comment Your Code**: Use comments to explain complex logic or the purpose of functions. Avoid obvious comments.

2. **JSDoc**: Document your functions, classes, and modules using JSDoc to improve code understanding and maintenance.

   ```typescript
   /**
    * Calculates the sum of two numbers.
    * @param a - The first number.
    * @param b - The second number.
    * @returns The sum of the two numbers.
    */
   function add(a: number, b: number): number {
       return a + b;
   }
   ```

## Resources

- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)
- [TypeScript Best Practices](https://basarat.gitbook.io/typescript/)
- [TypeScript Style Guide](https://github.com/typescript-eslint/typescript-eslint/blob/main/packages/eslint-plugin/docs/rules/naming-convention.md)
- [MDN Web Docs](https://developer.mozilla.org/en-US/)

Feel free to contribute to this guide or suggest improvements. Happy coding!
```

This `README.md` file provides a thorough overview of TypeScript best practices and should be helpful for teams or individual developers looking to improve their TypeScript code quality.