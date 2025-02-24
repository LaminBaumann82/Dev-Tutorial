# C# Basics


This document provides an overview of the fundamental concepts in C# programming, including variables, data types, control structures, and more.

If you run in to problems or unexpected behaviour please check out the [SetUpVisualStudio](SetUpVisualStudio.md) Tutorial.



## Table of Content
[Variables](#variables)
   - [Declaration and Initialization](#declaration-and-initialization)
   - [Data Types](#data-types)
       - [Integer](#integer)
       - [Float and Double](#float-double)
       - [String](#string)
       - [Boolean](#boolean)
       - [Date/Time](#date/time)
   - [Handling](#handling)
       - [Casting](#casting)
       - [Parsing](#parsing)
       - [Implicit vs Explicit](#implicit-vs-explicit)

[Control Structures](#control-structures)
   - [If-Else](#if-else)
   - [Switch](#switch)
   - [Loops](#loops)
   - [Ternary Operator](#ternary-operator)
   - [Expressions](#expressions)

[Functions](#functions)
   - [Method vs Function](#method-vs-function)
   - [Arguments and Parameters](#arguments-and-parameters)
   - [Return type](#return-type)
     
[Arrays](#arrays)

[Lists](#lists)

[Dictionaries](#dictionaries)

[Classes and Objects](#classes-and-objects)
   - [Fields vs Properities](#fields-vs-properties)
   - [Static vs instances](#static-vs-instances)
   - [Constructors](#constructors)

[Exception Handling](#exception-handling)
   - [Try/Catch](#try/catch)
   - [Debugging](#debugging)

---

## Variables
Variables are used to store data that can be used and manipulated within a C# program. They act as placeholders for values.

### Declaration and Initialization
In C#, variables must be declared with a type, and they can optionally be initialized with a value.
```csharp
int age = 25; // Declaration and initialization
string name = "Alice"; // String variable
bool isStudent = true; // Boolean variable
```

### `var` vs Explicit Typing
C# allows variables to be defined with explicit types (e.g., `int`, `string`) or with `var`, which uses implicit typing.

- **Explicit Typing**: The type is declared directly, providing clarity.
  ```csharp
  int number = 10; // Explicitly an integer
  string greeting = "Hello"; // Explicitly a string
  ```

- **Implicit Typing**: Using `var` infers the type based on the assigned value.
  ```csharp
  var number = 10;      // Inferred as int
  var greeting = "Hi";  // Inferred as string
  ```

> **Note**: `var` requires initialization at the time of declaration. The type cannot change once inferred.

---

## Data Types
C# has several data types to handle different kinds of data.

- **Integer (int)**: Stores whole numbers (e.g., 5, -42).
  ```csharp
  int count = 42;
  ```

- **Floating Point (float, double)**: Stores numbers with decimals.
  - `float` - 32-bit floating-point (suffix with `f`).
    ```csharp
    float height = 5.9f; // Using 'f' to indicate float
    ```
  - `double` - 64-bit floating-point, typically used for more precision.
    ```csharp
    double distance = 123.456;
    ```

- **String**: Represents sequences of characters.
  ```csharp
  string message = "Hello, World!";
  ```

- **Boolean (bool)**: Holds `true` or `false` values.
  ```csharp
  bool isActive = true;
  ```

### Numeric Type Comparison
- **int** is used for integer values.
- **float** and **double** are used for floating-point values, with **double** offering more precision. Always prefer `double` for calculations that require decimal precision.

---

## Control Structures
Control structures are used to control the flow of execution in a program.

### If-Else
The `if` statement checks a condition and executes the code block if the condition is `true`. Optionally, an `else` block can be included for cases where the condition is `false`.

```csharp
int number = 10;
if (number > 5) {
    Console.WriteLine("Number is greater than 5");
} else {
    Console.WriteLine("Number is 5 or less");
}
```

### Switch
The `switch` statement is useful for evaluating a variable against multiple possible values.

```csharp
int day = 3;
switch (day) {
    case 1:
        Console.WriteLine("Monday");
        break;
    case 2:
        Console.WriteLine("Tuesday");
        break;
    case 3:
        Console.WriteLine("Wednesday");
        break;
    default:
        Console.WriteLine("Another day");
        break;
}
```

### Loops
Loops allow for repetitive execution of code blocks.

#### For Loop
The `for` loop is commonly used when the number of iterations is known.

```csharp
for (int i = 0; i < 5; i++) {
    Console.WriteLine("Iteration " + i);
}
```

#### While Loop
The `while` loop repeats as long as a specified condition is true.

```csharp
int count = 0;
while (count < 5) {
    Console.WriteLine("Count is " + count);
    count++;
}
```

#### Do-While Loop
The `do-while` loop is similar to `while` but guarantees at least one iteration, as the condition is checked at the end.

```csharp
int count = 0;
do {
    Console.WriteLine("Count is " + count);
    count++;
} while (count < 5);
```
### Ternary Operator
### Expressions
---

## Functions
Functions (or methods) are reusable blocks of code that perform a specific task. They help organize code, making it more modular and easier to maintain.

### Defining a Function
A function is defined with a return type, name, and parameters (if any).
```csharp
int Add(int a, int b) {
    return a + b; // Returns the sum of a and b
}
```

### Calling a Function
You can call a function and use its return value.
```csharp
int sum = Add(5, 3); // sum now holds the value 8
Console.WriteLine("Sum is: " + sum);
```
### Method vs Function
### Arguments and Parameters
### Return type
---

## Arrays
Arrays are collections of variables that hold multiple values of the same type.

### Declaring and Initializing an Array
```csharp
int[] numbers = new int[5]; // An array of 5 integers
numbers[0] = 10; // Assigning values
numbers[1] = 20;
```

### Initializing an Array with Values
```csharp
int[] numbers = { 1, 2, 3, 4, 5 }; // Array initialized with values
```

### Iterating Through an Array
You can use a loop to access each element of an array.
```csharp
for (int i = 0; i < numbers.Length; i++) {
    Console.WriteLine(numbers[i]);
}
```
## Lists
## Dictionaries
## 
---

## Classes and Objects
C# is an object-oriented programming language. This means it uses classes to define objects and their behaviors.

### Defining a Class
```csharp
class Car {
    public string Make { get; set; }
    public string Model { get; set; }

    public void DisplayInfo() {
        Console.WriteLine($"Car: {Make} {Model}");
    }
}
```

### Creating an Object
```csharp
Car myCar = new Car(); // Creating an object of the Car class
myCar.Make = "Toyota"; // Setting properties
myCar.Model = "Corolla";
myCar.DisplayInfo(); // Calling a method
```
### Fields vs Properties
### Static vs Instances
### Constructors
---

## Exception Handling
Exception handling is a way to respond to runtime errors in a controlled manner. It prevents the program from crashing and allows for graceful error management.
### Try/Catch
### Try-Catch Block
You can use `try` to write code that might throw an exception and `catch` to handle that exception.

```csharp
try {
    int[] numbers = { 1, 2, 3 };
    Console.WriteLine(numbers[5]); // This will throw an exception
} catch (IndexOutOfRangeException ex) {
    Console.WriteLine("Error: " + ex.Message);
}
```
### Debugging
---

## Summary
This guide introduced basic C# concepts:
- Declaring variables using explicit and implicit typing.
- Working with common data types: `int`, `float`, `string`, and `bool`.
- Implementing control structures such as `if-else`, `switch`, and loops (`for`, `while`, `do-while`).
- Understanding functions for reusable code.
- Utilizing arrays for collections of data.
- Exploring classes and objects in C#.
- Managing errors with exception handling.

With these basics, you can start creating simple C# applications and build a foundation for more advanced programming concepts.
