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
- [Access Modifiers](#access-modifiers)
- [Inheritance](#inheritance)
- [Interfaces](#interfaces)
- [Abstract Classes](#abstract-classes)
- [Virtual](#virtual)
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

## Handle Multiple Projects and external Libraries
- [Multi-project](#multi-project)
- [Internal Access Modifier](#internal-access-modifier)
- [NuGet Packages](#nuget-packages)

## Async, Parallel and Multi-Threading
- [Threads](#threads)
- [Background Workers](#background-workers)
- [Task Objects](#task-objects)
- [Async/Await](#asyncawait)
- [Cancellation Token](#cancellation-token)

---

## Reference Types and Value Types
Classes are reference types in C#. The primitive types like int, float, double, string, bools are value types.
When we use reference types we are passing around a reference to that object in the memory.

---

### Basics Of Classes and Value Types

In C#, data types are categorized into two main groups: reference types and value types. Understanding the distinction between these two types is crucial for effective programming, as it influences how data is stored, accessed, and modified.



#### Reference Types
Reference types are stored in the heap memory and hold a reference (or pointer) to the actual data. When a reference type is passed to a method, a reference to the original object is sent, allowing modifications within the method to affect the original object. Common examples of reference types in C# include classes, interfaces, delegates, records arrays, and strings.

In the provided code, the list myList is a reference type, which demonstrates that modifications made within the ModifyListReference method affect the original list. This behavior emphasizes the need to understand how reference types work when designing applications.



#### Value Types
Value types, on the other hand, are stored directly in the stack memory and contain the actual data. When a value type is passed to a method, a copy of the data is made. As a result, changes made to the parameter within the method do not affect the original value outside of the method. Common examples of value types include primitive data types such as integers, floats, bool, char and structs.

In the example, the string myString represents a value type, showcasing how the ModifyValue method does not alter the original string when passed as an argument. Additionally, the code demonstrates how the ref keyword can be used to pass a value type by reference, allowing the method to modify the original data.



#### Conclusion
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

---

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

---

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

---

### Enum vs Struct
Enums and structs are both useful constructs in C#, but they serve different purposes and are used in different scenarios. Here’s a brief overview of when to use each:

---

When to Use Enums
Fixed Set of Related Constants: Use enums when you need a named group of related constants that represent discrete values. For example, days of the week, colors, or states of an application.

Readability and Maintainability: Enums improve code readability and maintainability. Instead of using magic numbers or strings, enums provide meaningful names, making the code easier to understand.

Type Safety: Enums provide type safety by restricting the variable to one of the predefined values. This helps prevent errors associated with using invalid values.

Comparative Operations: Enums can be easily compared using equality or other operators, making them suitable for situations where you need to switch or compare discrete values.

---

When to Use Structs
Grouping Related Data: Use structs when you need to group multiple related fields into a single entity. For example, a struct could represent a point in 2D space with X and Y coordinates.

Value Type Behavior: Structs are value types, which means they are copied when passed around. Use them when you want the value semantics (copying behavior) to prevent unintended modifications.

Simple Data Structures: Structs are best suited for small, simple data structures. Avoid using structs for large or complex types, as copying them can incur performance overhead.

Immutability: If you want to create an immutable type (i.e., its values cannot be changed after creation), structs are a good choice when combined with read-only properties.

---

Summary
Use Enums when you need a fixed set of related named constants for better readability and type safety.
Use Structs when you want to encapsulate related data in a single value type, especially for small, simple data structures that benefit from value semantics.
In conclusion, enums and structs are both valuable in C# programming, but they should be chosen based on the specific requirements of the task at hand.

---

### Equality Problem

Compare Objects in c# can be challenging. if you want to campare two classes with the "==" operator the outcome might not be as you expected. 
```c#
public class ClassCompareTest
{
    public void RunExample()
    {
        Console.WriteLine("Testing comparison of two classes");
        // instantiated with constructor with explicite typing
        ExampleClass instanceOfExampleClass1 = new ExampleClass("TheTextValue", 123456);
        // instantiated with Object Initializer with implicite typing
        var instanceOfExampleClass2 = new ExampleClass
        {
            Number = 123456,
            Text = "TheTextValue",
        };
        Console.WriteLine("are those 2 classes equal when compared with \"==\"?");
        Console.WriteLine($"Answer: {instanceOfExampleClass1 == instanceOfExampleClass2}");
        Console.WriteLine("are those 2 classes equal when compared with .Equals()?");
        Console.WriteLine($"Answer: {instanceOfExampleClass1.Equals(instanceOfExampleClass2)}");
       
        Console.WriteLine("And with the standard values?");
        var instanceOfExampleClass3 = new ExampleClass();
        var instanceOfExampleClass4 = new ExampleClass();
        Console.WriteLine($"text of instanceOfExampleClass3 is: {instanceOfExampleClass3.Text}");
        Console.WriteLine($"number of instanceOfExampleClass3 is: {instanceOfExampleClass3.Number}");
        Console.WriteLine($"text of instanceOfExampleClass4 is: {instanceOfExampleClass4.Text}");
        Console.WriteLine($"number of instanceOfExampleClass4 is: {instanceOfExampleClass4.Number}");
        Console.WriteLine("Compare Classes: ");
        Console.WriteLine("are those 2 classes equal when compared with \"==\"?");
        Console.WriteLine($"Answer: {instanceOfExampleClass3 == instanceOfExampleClass4}");
        Console.WriteLine("are those 2 classes equal when compared with .Equals()?");
        Console.WriteLine($"Answer: {instanceOfExampleClass3.Equals(instanceOfExampleClass4)}");
    }
    public class ExampleClass
    {
        public string Text { get; set; } = "standard Value";
        public int Number { get; set; } = 0;

        public ExampleClass() 
        { 
        }   
        public ExampleClass(string inputText, int inputNumber)
        {
            Text = inputText;
            Number = inputNumber;
        }
    }

}
```
As you can see comparison can be realy chanllanging with objects. You can run into false results if you don't pay attantion or put too less effort in the process. But help is on the way.
---

### Records

Records can help with the Equality Problem! A Record is a reference type but can be read / write like values. It is often used where DTO's (Data Transfer Objects) are needed.  Lets have a look.

```c#
// define a Record with properties. The init keyword is used to make the propertie imutalble. it is set once at object creation. Afterwards the value can not be changed. 
public Record MyRecord1()
{
    public int number { get; init; }
    public string text { get; init; }
}

// we can simplify this notation. note that even if we dont specificly use the keyword init is is still imutable. 
public Record MyRecord2(int number, string text);

// it is possible to mix this two variants like this:
public Record MyRecord3(int number, string text)
{
    public string additionalText { get; set; }
}






```

So with this new type we can compare a reference type in the same way we compare a value type.

```c#

MyRecord1 myfirstRecord = new(1, "aaa");



// define a Record with properties
public Record MyRecord1()
{
    public int number { get; set; }
    public string text { get; set; }
}

// we can simplify this notation:
public Record MyRecord2(int number, string text);

// it is possible to mix this two variants like this:
public Record MyRecord3(int number, string text)
{
    public string additionalText { get; set; }
}
```
---

## OOP Object-Oriented Programming
### Access Modifiers

In C# are multiple Access modifiers which we will look at in deept. 

#### Public
 Code in any assembly can access this type or member. The accessibility level of the containing type controls the accessibility level of public members of the type.

```c#
public class Public
{
    public void RunExample()
    {
        OtherClass.Main(); // run the main method from OtherClass.
    }

    public class SomeClass
    {
        public int Number = 133; // a public integer that can be accessed in another class.
    }
    
    public class OtherClass
    {
        public static void Main()
        {
            SomeClass myClass = new SomeClass(); // Create an instance of SomeClass.
            Console.WriteLine($"The value from \"Number\" in SomeClass is: {myClass.Number}"); // access the public integer from the class.
        } 
    }
}

```

---

#### Private
 Only code declared in the same class or struct can access this member.

```c#
public class Private
{
    public void RunExample()
    {
        OtherClass.Main();
    }
    public class ClassWithPrivate
    {
        private int Number1 = 1;
        public int Number2 = 2;
    }
    public class OtherClass
    {
        public static void Main()
        {
            ClassWithPrivate myClass = new ClassWithPrivate(); // Create an instance of SomeClass.
            
            // access the private integer from the class is not possible due to the private keyword.
            // Console.WriteLine($"The value from \"Number1\" in SomeClass is: {ClassWithPrivate.Number1}"); // this will not compile.
            
            Console.WriteLine($"The value from \"Number2\" in SomeClass is: {myClass.Number2}"); // access the public integer from the class works
        }
    }
}
```


---

#### Protected
Only code in the same class or in a derived class can access this type or member.

```c#

public class Protected
{
    public void RunExample()
    {
        Console.WriteLine("Output from OtherClass.Main()`: ");
        OtherClass.Main();
        Console.WriteLine("Output from DerivedClass.Main()`: ");
        DerivedClass.Main();
    }
    public class ClassWithProtected
    {
        private int Number1 = 1;
        public int Number2 = 2;
        protected int Number3 = 3;
    }
    public class OtherClass 
    {
        public static void Main()
        {
            ClassWithProtected myClass = new ClassWithProtected(); // Create an instance of SomeClass.

            // Access the private integer from the class is not possible due to the private keyword.
            // Console.WriteLine($"The value from \"Number1\" in ClassWithProtected is: {ClassWithPrivate.Number1}"); // this will not compile.

            Console.WriteLine($"The value from \"Number2\" in ClassWithProtected is: {myClass.Number2}"); // access the public integer from the class works

            // Access the protected integer is not possible due to the protected keyword and the OtherClass is not derived from ClassWith Proteced.
            // Console.WriteLine($"The value from \"Number3\" in ClassWithProtected is: {myClass.Number3}");
        }
    }

    public class DerivedClass : ClassWithProtected
    {
        public int AnotherNumber3;
        public DerivedClass()
        {
            // We can access the private Number3 directly in the constructor of the DerivedClass. 
            AnotherNumber3 = base.Number3; // "base" is greyed out because C# already knows that Number3 is from the base class.
            // In this situation it is better to write the following:
            AnotherNumber3 = Number3;
        }
    
        public static void Main()
        {
            ClassWithProtected myClass2 = new ClassWithProtected(); // Create an instance of ClassWithProtected.

            // Access the private integer from the class is not possible due to the private keyword.
            // Console.WriteLine($"The value from \"Number1\" in ClassWithProtected is: {myClass2.Number1}"); // This will not compile.

            Console.WriteLine($"The value from \"Number2\" in ClassWithProtected is: {myClass2.Number2}"); // Access the public integer from the class works.

            // This wont work:
            // ClassWithProtected myClassWithProtected = new ClassWithProtected();
            // Console.WriteLine($"The value from \"Number3\" in ClassWithProtected is: {classWithProtected.Number3}");

            // Access the protected "Number3" only works in the derived class.
            DerivedClass myDerivedClass = new DerivedClass(); // Create an instance from the DerivedClass witch is derived from ClassWithProtected
            Console.WriteLine($"The value from \"Number3\" in ClassWithProtected is: {myDerivedClass.Number3}"); 
        }
    }
}
```

---

#### Internal
Only code in the same assembly can access this type or member.


The class with a protected and an internal property is very simple:
```c#
public class Internal
{
    protected int Number1 = 1;
    internal int Number2 = 2;
}
```

In the same assembly with non derived class:
```c#
public class Internal2
{
    public void RunExample()
    {
        Internal myInternal = new Internal();
        // We instantiate Internal itself but we don't have access to the protected Property. Only in derived classes.
        //Console.WriteLine($"The protected number from the Internal class is: {myInternal.Number1}");
        // In the same assembly we instantiate the Internal class and we have access to the internal Property.
        Console.WriteLine($"The internal number from the Internal class is: {myInternal.Number2}");
    }
}
```

In the same assembly with a derived class:
```c#
public class Internal3 : Internal
{
    public void RunExample()
    {
        Internal3 myInternal3 = new Internal3();
        // Because we are in the same assembly and the Internal3 class is derived from Internal we have access to both.
        Console.WriteLine($"The protected number from the Internal class is: {myInternal3.Number1}");
        Console.WriteLine($"The internal number from the Internal class is: {myInternal3.Number2}");
    }
}
```

In another assembly with a derived class:
```c#
public class InternalInOtherAssembly : Internal
{
    public void RunExample()
    {
        InternalInOtherAssembly myInternal2 = new InternalInOtherAssembly();
        // In this case we have access to the protected Property because we derive from Internal but since we are in another assembly
        // we don't have access to the internal.
        Console.WriteLine($"The protected number from the Internal class is: {myInternal2.Number1}");
        // This wont compile:
        // Console.WriteLine($"The internal number from the Internal class is: {myInternal2.Number2}");
    }
}
```

---

#### Protected internal
Only code in the same assembly or in a derived class in another assembly can access this type or member.

---

#### Private internal
Only code in the same assembly and in the same class or a derived class can access the type or member.

---

#### File
Only code in the same file can access the type or member.

---

#### Overview
| Caller's location                   | `public` | `protected internal` | `protected` | `internal` | `private protected` | `private` | `file` |
|-------------------------------------|----------|-----------------------|-------------|------------|----------------------|-----------|--------|
| Within the file                     | ✔️       | ✔️                    | ✔️          | ✔️         | ✔️                   | ✔️        | ✔️     |
| Within the class                    | ✔️       | ✔️                    | ✔️          | ✔️         | ✔️                   | ✔️        | ❌     |
| Derived class (same assembly)       | ✔️       | ✔️                    | ✔️          | ✔️         | ✔️                   | ❌        | ❌     |
| Non-derived class (same assembly)   | ✔️       | ✔️                    | ❌          | ✔️         | ❌                   | ❌        | ❌     |
| Derived class (different assembly)  | ✔️       | ✔️                    | ✔️          | ❌         | ❌                   | ❌        | ❌     |
| Non-derived class (different assembly) | ✔️    | ❌                    | ❌          | ❌         | ❌                   | ❌        | ❌     |


This whole Article about access modifiers is from [Learn Microsoft](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/access-modifiers)

---

### Inheritance

---

### Interfaces
"In C#, classes can implement multiple interfaces. An interface in C# is a contract that defines a set of methods, properties, events, or indexers without providing their implementations. It specifies what a class should do, but not how it should do it. Any class that implements an interface agrees to provide the code for the members defined by that interface. This allows different classes to follow the same structure, enabling polymorphism and helping to build flexible, modular code.
```c#


public class InterfaceExample
{
    public void RunExample()
    {
        Console.WriteLine("Start of interface example:");
        House myHouse = new House(4, false); // set number of windows to 4 and inhabitance to false
        Console.WriteLine($"This House has {myHouse.NumberOfWindows} windows.");
        Console.WriteLine("Check window status:");
        CheckWindows(myHouse);
        Console.WriteLine("Open window 3:");
        myHouse.OpenWindow(3);
        CheckWindows(myHouse);
        Console.WriteLine("Open window 1:");
        myHouse.OpenWindow(1);
        CheckWindows(myHouse);
        Console.WriteLine("Close window 3:");
        myHouse.CloseWindow(3);
        CheckWindows(myHouse);
        Console.WriteLine("Inhabitance");
        Console.WriteLine("Check if this house is inhabited:");
        CheckInhabitance(myHouse);
        Console.WriteLine("Peter moves in.");
        myHouse.MoveIn();
        CheckInhabitance(myHouse);
        Console.WriteLine("Peter moves out.");
        myHouse.MoveOut();
        CheckInhabitance(myHouse);
        Console.WriteLine("Call the MoveOut() method again:");
        myHouse.MoveOut(); 

    }

    void CheckWindows(IHasWindows windows)
    {
        for (int i = 0; i < windows.NumberOfWindows; i++)
        {
            Console.WriteLine($"Window {i} is open: {windows.IsWindowOpen(i)}");
        }
    }
    void CheckInhabitance(IIsInhabited building)
    {
        Console.WriteLine($"is this house inhabited? {building.IsInhabited}");
    }
    public class House : IHasWindows, IIsInhabited
    {
        private readonly bool[] _windows; // readonly makes sure the array is only set once. the fields in the array can still be modified.
        // next line: not necessary because it is implementet with a private setter on line 58.
        //private readonly bool _isInhabited; 

        public House(int numberOfWindows, bool isInhabited)
        {
                
            _windows = new bool[numberOfWindows]; // Create a new boolean array with all values initially set to false.
            //_isInhabited = isInhabited;    _isInhabited field is not needed because the inhabitance status is managed by the IsInhabited property.
            IsInhabited = isInhabited;
                
        }
        public int NumberOfWindows => _windows.Length;
        public bool IsInhabited { get; private set; } // The Property from the interface was get only. we added the private set so we can change the status. 
        public void OpenWindow(int index)
        {
            // We are not checking if index is out of range. This could lead to an IndexOutOfRangeException if an invalid index is passed.
            //_windows[index] = true;
            //Better solution:
            if (index >= 0 && index < _windows.Length)
                _windows[index] = true;
            else
                Console.WriteLine($"Invalid window index: {index}");
        }
        public void CloseWindow(int index)
        {
            if (index >= 0 && index < _windows.Length)
                _windows[index] = false;
            else
                Console.WriteLine($"Invalid window index: {index}");
        }
        public bool IsWindowOpen(int index)
        {
            if (index >= 0 && index < _windows.Length)
                return _windows[index];
            else
            {
                Console.WriteLine($"Invalid window index: {index}");
                return false; // Or another default, depending on desired behavior
            }
        }

        public void MoveIn()
        {
            if(IsInhabited)
            {
                Console.WriteLine("House is already inhabited");
                return; 
            }
            IsInhabited = true;
            Console.WriteLine("Someone moved in.");
        }

        public void MoveOut()
        {
            if (!IsInhabited)
            {
                Console.WriteLine("House is already empty.");
                return;
            }
            IsInhabited = false;
            Console.WriteLine("Someone moved out.");
        }
    }

    public interface IHasWindows
    {
        int NumberOfWindows { get; }
        void OpenWindow(int index);
        void CloseWindow(int index);
        bool IsWindowOpen(int index);
    }

    public interface IIsInhabited
    {
        bool IsInhabited { get;}
        void MoveIn();
        void MoveOut();
    }
}


```

---

### Abstract Classes
Abstract Classes in C# can define some base functionality. They can not be instantiated. Like Interfaces a class can inherit from multiple abstract classes or interfaces. An Abstract class can inherit from another Abstract Class. 
```c#
public class AbstractClassesExample
{
    public void RunExample()
    {
        // Instantiate MyClass, which directly implements AbstractBaseClass and IMyInterface
        MyClass myClass = new MyClass();
        myClass.Print();               // Calls Print() from AbstractBaseClass
        myClass.AbstractPrint();       // Calls overridden AbstractPrint() in MyClass
        myClass.PrintInterface();      // Calls PrintInterface() from MyClass, as required by IMyInterface

        // Instantiate FinalClass, which inherits from DerivedAbstractClass
        FinalClass finalClass = new FinalClass();
        finalClass.Print();            // Calls Print() from AbstractBaseClass
        finalClass.AbstractPrint();    // Calls overridden AbstractPrint() in FinalClass
        finalClass.PrintInterface();   // Calls overridden PrintInterface() in FinalClass
    }

    // Define an abstract base class with one concrete method and one abstract method
    public abstract class AbstractBaseClass
    {
        // Concrete method with implementation
        public void Print()
        {
            Console.WriteLine("This is the Print() from the AbstractBaseClass");
        }

        // Abstract method - must be implemented in any non-abstract derived class
        public abstract void AbstractPrint();
    }

    // MyClass is a concrete class that inherits from AbstractBaseClass and implements IMyInterface
    public class MyClass : AbstractBaseClass, IMyInterface
    {
        // Override the abstract method from AbstractBaseClass
        public override void AbstractPrint()
        {
            Console.WriteLine("This is the AbstractPrint() from the override in MyClass");
        }

        // Implement the method required by IMyInterface
        public void PrintInterface()
        {
            Console.WriteLine("This is the PrintInterface() from MyClass");
        }
    }

    // Define an abstract derived class that inherits from AbstractBaseClass and implements IMyInterface
    // This class does not need to implement AbstractPrint() because it remains abstract
    public abstract class DerivedAbstractClass : AbstractBaseClass, IMyInterface
    {
        // Define PrintInterface as abstract, leaving the implementation to concrete subclasses e.g. Our FinalClass
        public abstract void PrintInterface();
    }

    // FinalClass is a concrete class that inherits from DerivedAbstractClass
    public class FinalClass : DerivedAbstractClass
    {
        // Override the abstract method from AbstractBaseClass
        public override void AbstractPrint()
        {
            Console.WriteLine("This is the AbstractPrint() from the override in FinalClass");
        }

        // Override the abstract PrintInterface() method from DerivedAbstractClass
        public override void PrintInterface()
        {
            Console.WriteLine("This is the PrintInterface() from the override in FinalClass");
        }
    }

    // Define an interface with a single method that classes must implement
    public interface IMyInterface
    {
        void PrintInterface();
    }
}

```

---

### Virtual
The keyword virtual provides a hybrid approach between fully abstract and non-abstract methods. While abstract methods are declared without a function body and must be implemented in derived classes, virtual allows us to define a complete function in the base class that can optionally be overridden in derived classes. This means we can either use the original function as defined in the base class or override it with new behavior in a derived class.

```c#

public class Virtual
{
    public void RunExample()
    {
        FirstDerivedClass myFirstDerivedClass = new FirstDerivedClass();
        myFirstDerivedClass.PrintInBase();          // The override in the derived class.
        myFirstDerivedClass.VirtualPrintInBase();   // The override in the derived class.
        SecondDerivedClass mySecondDerivedClass = new SecondDerivedClass();
        mySecondDerivedClass.PrintInBase2();        // The override in the derived class.
        mySecondDerivedClass.VirtualPrintInBase2(); // The original method from the base class.
    }

    public abstract class FirstBaseClass
    {
        public abstract void PrintInBase(); // Abstract method without function body

        public virtual void VirtualPrintInBase() // Virtual method with function body
        {
            Console.WriteLine("VirtualPrintInBase");
        }
    }

    public class FirstDerivedClass : FirstBaseClass
    {
        public override void PrintInBase() // Override the abstract method as we must.
        {
            Console.WriteLine("PrintInBase() override in FirstDerivedClass");  
        }

        public override void VirtualPrintInBase() // Override the virtual method as we can.
        {
            Console.WriteLine("VirtualPrintInBase override in FirstDerivedClass");
        }
    }

    public abstract class SecondBaseClass
    {
        public abstract void PrintInBase2();
        public virtual void VirtualPrintInBase2() 
        {
            Console.WriteLine("VirtualPrintInBase");
        }
    }

    public class SecondDerivedClass : SecondBaseClass
    {
        public override void PrintInBase2() // And again the abstract method override
        {
            Console.WriteLine("PrintInBase() override in SecondDerivedClass");

            // In this class we don't have an override for the virtual method. We use the defined method body from the base class.
        }
    }
}


```

---

### Composition

---

### Generics

---

### Tuples

---

## Binary and String Data



### Encoding / Decoding

---

### Streams

---

### Reading / Writing Files

---

### Using and Disposable

---

### XML and JSON

---

## Methods and Functions



### Callbacks and Delegates

---

### Extension Methods

---

### LINQ

---

### Lazy

---

### Events

---

## Multi-project 

---

## Internal Access Modifier

---

## NuGet Packages

---

## Async, Parallel and Multi-Threading



### Threads

---

### Background Workers

---

### Task Objects

---

### Async/Await

---

### Cancellation Token


