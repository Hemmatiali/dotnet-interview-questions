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

## Q2. What are the four main principles of OOP?

**Question:**  
What are the four main principles of Object-Oriented Programming (OOP)?

**Answer:**  
The four main principles of OOP are **Encapsulation**, **Inheritance**, **Polymorphism**, and **Abstraction**. Below are precise definitions and idiomatic C# examples for each.

### 1. Encapsulation  
Encapsulation means **hiding an object’s internal state** and exposing only what’s necessary via a controlled API (properties/methods).  
- Achieved with **access modifiers** (`private`, `protected`, `internal`, `public`).  
- Use **properties** (with validation) instead of exposing public fields.  
- Helps enforce invariants and prevents accidental misuse.

**Example:**
```csharp
public class BankAccount
{
    private decimal balance; // hidden from outside

    public void Deposit(decimal amount)
    {
        if (amount > 0)
            balance += amount;
    }

    public decimal GetBalance()
    {
        return balance;
    }
}
```

### 2. Inheritance  
Inheritance allows a derived class to reuse and extend behavior of a base class.
- C# supports single inheritance of classes but allows implementing multiple interfaces.
- Use virtual/override to customize behavior; use sealed to stop further overrides when needed.

**Example:**
```csharp
public class Animal
{
    public void Eat() => Console.WriteLine("Eating...");
}

public class Dog : Animal
{
    public void Bark() => Console.WriteLine("Bark!");
}
```

### 3. Polymorphism  
Polymorphism lets the same operation behave differently depending on the actual object type.
- Runtime polymorphism via virtual (base) and override (derived).
- Enables designing to abstractions (base classes/interfaces) rather than concrete implementations.

**Example:**
```csharp
public class Animal
{
    public virtual void Speak() => Console.WriteLine("Animal sound");
}

public class Dog : Animal
{
    public override void Speak() => Console.WriteLine("Bark");
}

public class Cat : Animal
{
    public override void Speak() => Console.WriteLine("Meow");
}

// Usage:
var animals = new List<Animal> { new Dog(), new Cat(), new Animal() };
foreach (var a in animals) a.Speak(); 
// Output:
// Bark
// Meow
// Animal sound

```
### 4. Abstraction  
Abstraction focuses on what an object does, not how it does it.
- Model contracts with interfaces or abstract classes.
- Hide implementation details, expose essential operations.

**Example:**
```csharp
public interface IPaymentProcessor
{
    void ProcessPayment(decimal amount);
}

public class CreditCardProcessor : IPaymentProcessor
{
    public void ProcessPayment(decimal amount)
    {
        // Implementation details hidden from callers
        Console.WriteLine($"Paid {amount:C} with credit card.");
    }
}

public class BitcoinProcessor : IPaymentProcessor
{
    public void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Paid {amount:C} with Bitcoin.");
    }
}

// Usage via abstraction:
void Checkout(IPaymentProcessor processor, decimal amount) => processor.ProcessPayment(amount);

// Caller decides implementation:
Checkout(new CreditCardProcessor(), 49.99m);
Checkout(new BitcoinProcessor(),    49.99m);

```

```csharp
public abstract class AnimalBase
{
    public abstract void MakeSound(); // no default implementation
}

public class Bird : AnimalBase
{
    public override void MakeSound() => Console.WriteLine("Tweet");
}

```





