# C# Advanced

# Table of contents

## Prerequisites
To run the code examples in this Tutorial you will need to take some steps. If you don't allready done it please finish the Set up Visual Studio Tutorial. 
[Set up Visual Studio](/set-up-visual-studio.md)


## Reference Types and Value Types
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

### Enums
The use of Enums should be only in the case that you have a fixed set of values witch "never" change. enums look like strings but in fact they are numeric values.

  ```
  public Class Enums
  {
    public void Run()
    {
      // we can cast an enum to a int:
      int january = (int)MonthOFYear.January;

      // but we can not cast an enum to a string. following code will not compile:
      // string january = (string)MonthsOfYear.January;

      // this might be iritating because when we write those enums to the console they look like a string.
      Console.WriteLine($"Enum Value not casted: {MonthsOfYear.January}");
      Console.WriteLine($"Enum Value casted: {(int)MonthsOfYear.January}");

      // To convert an enum to a string representation, we use the ToString() method.
      string januaryString = MonthsOfYear.January.ToString();
      
      // To convert a string back to an enum value, we use the Enum.Parse method.
      MonthsOfYear januaryEnum = (MonthsOfYear)Enum.Parse(typeof(MonthsOfYear), "January");
      MonthsOfYear januaryEnum2 = Enum.Parse<MonthsOfYear>("January");
      
      // There’s also a TryParse variation for safer parsing.
      MonthsOfYear januaryEnum3;
      bool parseSucceeded = Enum.TryParse("January", out januaryEnum3);
      Console.WriteLine($"Enum {(parseSucceeded ? "Was Parsed" : "Was Not Parsed")}: {januaryEnum3}");
      
      // We can retrieve all values of the enum using Enum.GetValues.
      Console.WriteLine("All Enum Values:");
      foreach (MonthsOfYear month in Enum.GetValues(typeof(MonthsOfYear)))
      {
          Console.WriteLine($"Enum Value: {(int)month}");
      }
      
      // Enum.GetNames lets us get a list of all the enum names.
      Console.WriteLine("All Enum Names:");
      foreach (string month in Enum.GetNames(typeof(MonthsOfYear)))
      {
          Console.WriteLine($"Enum Name: {month}");
      }
      
      // It’s possible to cast an integer to an enum, even if the integer doesn’t match any valid enum member.
      MonthsOfYear invalidMonth = (MonthsOfYear)13;
      Console.WriteLine($"Invalid Enum Value: {invalidMonth}");
      
      // With the [Flags] attribute and values set to powers of 2, we can combine enum flags.
      
      // We can combine flags like this:
      Permissions readWrite = Permissions.Read | Permissions.Write;
      Console.WriteLine($"RW: {readWrite}");
      
      // We can also check if a specific flag is set with bitwise operations.
      bool canRead = (readWrite & Permissions.Read) == Permissions.Read;
      bool canWrite = (readWrite & Permissions.Write) == Permissions.Write;
      bool canExecute = (readWrite & Permissions.Execute) == Permissions.Execute;
      Console.WriteLine($"Can Read: {canRead}");
      Console.WriteLine($"Can Write: {canWrite}");
      Console.WriteLine($"Can Execute: {canExecute}");

    }

    // define an enum:
    enum MonthOFYear
    {
      January,
      February,
      March,
      April,
      Mai,
      June,
      July,
      August,
      September,
      October,
      November,
      December
    }

    // another way to define an enum:
    enum MonthOFYear2
    {
      January = 1,
      February = 2,
      March = 3,
      April = 4,
      Mai = 5 ,
      June = 6,
      July = 7,
      August = 8,
      September = 9,
      October = 10,
      November = 11,
      December = 12
    }

    // enums can also be used as "flags" which means that we can combine them
    // using bitwise operators
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


