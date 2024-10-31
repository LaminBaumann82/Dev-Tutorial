# C# Advanced

# Table of contents

## Prerequisites
To run the code examples in this Tutorial you will need to take some steps. If you don't allready done it please finish the Set up Visual Studio Tutorial. 
[Set up Visual Studio](/SetUpVisualStudio.md)


## Reference Types and Value Types
- [Basics Of Classes and Value Types](#basics-of-classes-and-value-types)
- [Enums](#enums)
- [Structs](#structs)
- [Enum vs Struct](#enum-vs-struct)
- [Equality Problem](#equality-problem)
- [Records](#records)

## OOP Object-Oriented Programming
- [Inheritance](#inheritance)
- [Interfaces](#interfaces)
- [Abstract Classes](#abstract-classes)
- [Protected and Virtual](#protected-and-virtual)
- [Composition](#composition)
- [Generics](#generics)
- [Tuples](#tuples)

## Binary and String Data
- [Encoding / Decoding](#encoding--decoding)
- [Streams](#streams)
- [Reading / Writing Files](#reading--writing-files)
- [Using and Disposable](#using-and-disposable)
- [XML and JSON](#xml-and-json)

## Methods and Functions
- [Callbacks and Delegates](#callbacks-and-delegates)
- [Extension Methods](#extension-methods)
- [LINQ](#linq)
- [Lazy](#lazy)
- [Events](#events)

## [Multi-project](#multi-project)

## [Internal Access Modifier](#internal-access-modifier)

## [NuGet Packages](#nuget-packages)

## Async, Parallel and Multi-Threading
- [Threads](#threads)
- [Background Workers](#background-workers)
- [Task Objects](#task-objects)
- [Async/Await](#asyncawait)
- [Cancellation Token](#cancellation-token)


## Reference Types and Value Types
Classes are reference types in C#. The primitive types like int, float, double, string, bools are value types.
When we use reference types we are passing around a reference to that object in the memory.

### Basics Of Classes and Value Types

In C#, data types are categorized into two main groups: reference types and value types. Understanding the distinction between these two types is crucial for effective programming, as it influences how data is stored, accessed, and modified.

Reference Types
Reference types are stored in the heap memory and hold a reference (or pointer) to the actual data. When a reference type is passed to a method, a reference to the original object is sent, allowing modifications within the method to affect the original object. Common examples of reference types in C# include classes, interfaces, delegates, records arrays, and strings.

In the provided code, the list myList is a reference type, which demonstrates that modifications made within the ModifyListReference method affect the original list. This behavior emphasizes the need to understand how reference types work when designing applications.

Value Types
Value types, on the other hand, are stored directly in the stack memory and contain the actual data. When a value type is passed to a method, a copy of the data is made. As a result, changes made to the parameter within the method do not affect the original value outside of the method. Common examples of value types include primitive data types such as integers, floats, bool, char and structs.

In the example, the string myString represents a value type, showcasing how the ModifyValue method does not alter the original string when passed as an argument. Additionally, the code demonstrates how the ref keyword can be used to pass a value type by reference, allowing the method to modify the original data.

Conclusion
This code illustrates the fundamental differences between reference types and value types in C#. Understanding these concepts is essential for developers to manage memory efficiently and avoid unintended side effects in their applications.

```c#
public sealed class BasicsToClassesAndValueTypes
{
    public void ExecuteDemo()
    {
        // Classes are reference types in C#
        // Primitive types (like integers, floats, and booleans) are value types.

        // When we use a reference type, we are passing 
        // a reference to the object in memory.

        List<string> myList = new()
        {
            "Good",
            "Morning",
        };

        void ModifyListReference(List<string> list)
        {
            list.Add("From");
            list.Add("Lamin");
        }

        Console.WriteLine("Reference Before:");
        foreach (var element in myList)
        {
            Console.WriteLine(element);
        }

        ModifyListReference(myList);

        Console.WriteLine("Reference After:");
        foreach (var element in myList)
        {
            Console.WriteLine(element);
        }

        string myString = "Hello, Universe!";
        void ModifyValue(string value)
        {
            value = "Goodbye, Universe!";
        }

        Console.WriteLine("Value Before:");
        Console.WriteLine(myString);

        ModifyValue(myString);

        Console.WriteLine("Value After:");
        Console.WriteLine(myString);

        // We can pass a value type by reference using the ref keyword.
        void ModifyValueByRef(ref string value)
        {
            value = "Goodbye, Universe!";
        }

        Console.WriteLine("Value Before By Reference:");
        Console.WriteLine(myString);

        ModifyValueByRef(ref myString);

        Console.WriteLine("Value After By Reference:");
        Console.WriteLine(myString);
    }
}

```
### Enums
The use of Enums should only be in cases where you have a fixed set of values that "never" change. Enums look like strings, but in fact, they are numeric values.

  ```c#
 public class MonthEnumerations
{
    public void ExecuteDemo()
    {
        // Enum values are inherently numeric, allowing for casting to integers.
        int firstMonth = (int)MonthNames.January;

        // Note: Attempting to cast an enum to a string directly will result in a compilation error.
        // string firstMonthStr = (string)MonthNames.January; // This line is invalid.

        // When printed, enums appear similar to strings in the console output.
        Console.WriteLine($"Raw Enum Output: {MonthNames.January}");
        Console.WriteLine($"Numeric Enum Value: {(int)MonthNames.January}");

        // To convert an enum to its string representation, use the ToString() method.
        string monthAsString = MonthNames.January.ToString();

        // To convert a string back to an enum, utilize the Enum.Parse method.
        MonthNames parsedMonth = (MonthNames)Enum.Parse(typeof(MonthNames), "January");
        MonthNames parsedMonthDirectly = Enum.Parse<MonthNames>("January");

        // A safer option is to use TryParse for enum conversion.
        MonthNames attemptedParseMonth;
        bool wasParsed = Enum.TryParse("January", out attemptedParseMonth);
        Console.WriteLine($"Parsing Status: {(wasParsed ? "Successfully Parsed" : "Failed to Parse")}: {attemptedParseMonth}");

        // Retrieve all values from the MonthNames enum using Enum.GetValues.
        Console.WriteLine("Available Enum Values:");
        foreach (MonthNames month in Enum.GetValues(typeof(MonthNames)))
        {
            Console.WriteLine($"Enum Value: {(int)month}");
        }

        // Obtain all names associated with the MonthNames enum.
        Console.WriteLine("Available Enum Names:");
        foreach (string month in Enum.GetNames(typeof(MonthNames)))
        {
            Console.WriteLine($"Enum Name: {month}");
        }

        // An interesting behavior allows casting an integer to an enum, even if it's invalid.
        MonthNames invalidMonth = (MonthNames)13; // 13 is not a defined month.
        Console.WriteLine($"Invalid Month Enum: {invalidMonth}");

        // Using the [Flags] attribute allows us to work with enum values as bitwise flags.
        Permissions userPermissions = Permissions.Read | Permissions.Write;
        Console.WriteLine($"Combined Permissions: {userPermissions}");

        // Check if specific permissions are granted using bitwise operations.
        bool canRead = (userPermissions & Permissions.Read) == Permissions.Read;
        bool canWrite = (userPermissions & Permissions.Write) == Permissions.Write;
        bool canExecute = (userPermissions & Permissions.Execute) == Permissions.Execute;

        Console.WriteLine($"Permission to Read: {canRead}");
        Console.WriteLine($"Permission to Write: {canWrite}");
        Console.WriteLine($"Permission to Execute: {canExecute}");
    }

    // Define the MonthNames enum representing each month.
    enum MonthNames
    {
        January,
        February,
        March,
        April,
        May,
        June,
        July,
        August,
        September,
        October,
        November,
        December
    }

    // Another way to define the MonthNames enum with assigned integer values.
    enum MonthNamesWithValues
    {
        January = 1,
        February = 2,
        March = 3,
        April = 4,
        May = 5,
        June = 6,
        July = 7,
        August = 8,
        September = 9,
        October = 10,
        November = 11,
        December = 12
    }

    // Permissions can be defined as flags for more flexible handling of multiple values.
    [Flags]
    enum Permissions
    {
        None = 0,       // 0000 0000
        Read = 1,       // 0000 0001
        Write = 2,      // 0000 0010
        Execute = 4     // 0000 0100
    }
}

 ``` 
### Structs

Structs are user-defined types that are categorized as value types, which means they are stored directly in memory rather than through a reference. This distinction sets them apart from classes, which are reference types and involve a more complex memory allocation process.

Structs are commonly used to represent small, simple data structures, such as points in a coordinate system or colors, where performance and memory efficiency are essential. When a struct is passed to a method, a copy is made, which can help avoid unintentional modifications to the original data. This behavior makes structs suitable for scenarios where immutability or simplicity is desired.

Developers often choose structs to minimize the overhead associated with heap allocation and garbage collection. However, it’s important to consider that structs should be used judiciously, as they can also lead to performance issues if they are too large or complex due to the cost of copying.

In summary, structs in C# provide an efficient way to encapsulate small amounts of data while adhering to value semantics, making them a valuable tool in the programmer’s toolkit.


```c#
public class StructsDemo
{
    public void ExecuteDemo()
    {
        // A struct is classified as a value type, although it may resemble a class in appearance.

        // What distinguishes a struct from a class?
        // When should we choose a struct instead of a class?
        // The primary distinction is that structs are value types while classes are reference types.
        // - Structs are stored on the stack, while classes reside on the heap.
        // - When passed to a method, a struct is copied, whereas a class is passed by reference.

        // Example illustrating how a struct is copied when passed to a method:
        void UpdatePoint(Point point)
        {
            point.X = 111; // Modifying the copy will not affect the original.
            point.Y = 222; 
        }

        var initialPoint = new Point()
        {
            X = 123,
            Y = 456
        };
        
        Console.WriteLine(
            $"Initial Point before UpdatePoint: " +
            $"{initialPoint.X}, {initialPoint.Y}");
        UpdatePoint(initialPoint);
        Console.WriteLine(
            $"Initial Point after UpdatePoint: " +
            $"{initialPoint.X}, {initialPoint.Y}");

        // Structs can be misleading due to their similarity to classes,
        // leading to confusion about their usage.
        // Here are some guidelines:
        // - Use a struct for small, simple objects that you wish to pass by value.
        // - Choose a struct to avoid the overhead of heap allocation and garbage collection.
        // I often think of simple geometric types like a Point,
        // Color, Rectangle, or other geometric figures.
    }

    // Example of a struct definition
    public struct Point
    {
        public int X;
        public int Y;
    }

    // The same struct defined with properties for better encapsulation:
    public struct PointWithProperties
    {
        public int X { get; set; }
        public int Y { get; set; }
    }

    // The same struct but includes a constructor for initialization:
    public struct PointWithConstructor
    {
        public PointWithConstructor(int x, int y)
        {
            X = x;
            Y = y;
        }

        public int X { get; set; }
        public int Y { get; set; }
    }

    // A struct can also include methods, similar to classes:
    public struct PointWithMethod
    {
        public int X;
        public int Y;

        public void Move(int deltaX, int deltaY)
        {
            X += deltaX;
            Y += deltaY;
        }
    }
}

```
### Enum vs Struct
Enums and structs are both useful constructs in C#, but they serve different purposes and are used in different scenarios. Here’s a brief overview of when to use each:

When to Use Enums
Fixed Set of Related Constants: Use enums when you need a named group of related constants that represent discrete values. For example, days of the week, colors, or states of an application.

Readability and Maintainability: Enums improve code readability and maintainability. Instead of using magic numbers or strings, enums provide meaningful names, making the code easier to understand.

Type Safety: Enums provide type safety by restricting the variable to one of the predefined values. This helps prevent errors associated with using invalid values.

Comparative Operations: Enums can be easily compared using equality or other operators, making them suitable for situations where you need to switch or compare discrete values.

When to Use Structs
Grouping Related Data: Use structs when you need to group multiple related fields into a single entity. For example, a struct could represent a point in 2D space with X and Y coordinates.

Value Type Behavior: Structs are value types, which means they are copied when passed around. Use them when you want the value semantics (copying behavior) to prevent unintended modifications.

Simple Data Structures: Structs are best suited for small, simple data structures. Avoid using structs for large or complex types, as copying them can incur performance overhead.

Immutability: If you want to create an immutable type (i.e., its values cannot be changed after creation), structs are a good choice when combined with read-only properties.

Summary
Use Enums when you need a fixed set of related named constants for better readability and type safety.
Use Structs when you want to encapsulate related data in a single value type, especially for small, simple data structures that benefit from value semantics.
In conclusion, enums and structs are both valuable in C# programming, but they should be chosen based on the specific requirements of the task at hand.
### Equality Problem
### Records

## OOP Object-Oriented Programming
### Inheritance
### Interfaces
### Abstract Classes
### Protected and Virtual
### Composition
### Generics
### Tuples

## Binary and String Data
### Encoding / Decoding
### Streams
### Reading / Writing Files
### Using and Disposable
### XML and JSON

## Methods and Functions
### Callbacks and Delegates
### Extension Methods
### LINQ
### Lazy
### Events

## Multi-project 

## Internal access modifier

## NuGet Packages

## Async, Parallel and Multi-Threading
### Threads
### Background Workers
### Task Objects
### Async/Await
### Cancellation Token


