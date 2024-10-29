# C# Advanced

# Table of contents

## Prerequisites
To run the code examples in this Tutorial you will need to take some steps. If you don't allready done it please finish the Set up Visual Studio Tutorial. 
[Set up Visual Studio](/SetUpVisualStudio.md)


## Reference Types and Value Types
- [Basics Of Classes and Value Types](#basics-of-classes-and-value-types)
- [Enums](#enums)
- [Structs](#structs)
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
Reference types are stored in the heap memory and hold a reference (or pointer) to the actual data. When a reference type is passed to a method, a reference to the original object is sent, allowing modifications within the method to affect the original object. Common examples of reference types in C# include classes, arrays, and strings.

In the provided code, the list myList is a reference type, which demonstrates that modifications made within the ModifyListReference method affect the original list. This behavior emphasizes the need to understand how reference types work when designing applications.

Value Types
Value types, on the other hand, are stored directly in the stack memory and contain the actual data. When a value type is passed to a method, a copy of the data is made. As a result, changes made to the parameter within the method do not affect the original value outside of the method. Common examples of value types include primitive data types such as integers, floats, and structs.

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


