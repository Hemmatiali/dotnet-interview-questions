## Q1. What is OOP?

**Question:**  
What is OOP?

**Answer:**  
OOP stands for **Object-Oriented Programming**. It is a programming paradigm where code is organized into units called **objects**. Each object bundles together two things:

1. **Data (fields, properties, attributes)** – the information that describes the object.  
2. **Behavior (methods, functions)** – the actions that the object can perform.

This combination of data and behavior in one unit allows developers to model real-world entities more naturally in software.

For example, consider a simple class:

```csharp
public class Car
{
    // Data (state)
    public string Color { get; set; }
    public int Speed { get; set; }

    // Behavior (methods)
    public void Drive()
    {
        Console.WriteLine("The car is driving...");
    }

    public void Stop()
    {
        Console.WriteLine("The car has stopped.");
    }
}
```