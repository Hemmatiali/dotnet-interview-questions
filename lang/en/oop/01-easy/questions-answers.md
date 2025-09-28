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

## Q3. What are the key differences between an abstract class and an interface in C#?

**Question:**  
What are the key differences between an **abstract class** and an **interface** in C#, and when should I use each?

**Answer:**  
At a glance:

| Feature                               | Abstract Class                          | Interface                                                                 |
|---------------------------------------|-----------------------------------------|---------------------------------------------------------------------------|
| Can contain implementation?           | ✅ Yes (abstract **and** concrete)       | ⚠️ Historically **no**; **since C# 8** can have **default implementations** |
| Can have fields (state)?              | ✅ Yes (instance fields allowed)         | ❌ No instance fields                                                      |
| Constructors                          | ✅ Yes                                   | ❌ No instance constructors                                                |
| Inheritance / Implementation count    | ✅ Single base class only                | ✅ A class can implement **multiple** interfaces                           |
| Access modifiers on members           | ✅ Any (`public/protected/internal/...`) | ⚠️ Members are effectively part of a public contract (public surface)      |
| Use case                              | Share **state + base behavior**         | Define **capability/contract** across unrelated types                     |

**Guidance:**  
- Choose an **abstract class** when you need a shared base with **state, protected helpers, and partial implementation**.  
- Choose an **interface** when you want to model **capabilities** that many unrelated types can implement.  
- From **C# 8+**, interfaces can include **default interface methods**, but they still **cannot hold instance fields** and are best treated as contracts.

**Examples**

*Abstract class (shared state + behavior):*
```csharp
public abstract class Shape
{
    // Shared state
    protected double _x, _y;

    protected Shape(double x, double y)
    {
        _x = x; _y = y;
    }

    // Must be implemented by derived types
    public abstract double Area();

    // Optional shared behavior
    public virtual void Move(double dx, double dy)
    {
        _x += dx; _y += dy;
    }
}

public sealed class Circle : Shape
{
    private readonly double _r;

    public Circle(double x, double y, double r) : base(x, y) => _r = r;

    public override double Area() => Math.PI * _r * _r;
}
```
```csharp
public interface IPrintable
{
    void Print();
    // C# 8+: default implementation possible
    // void Print() => Console.WriteLine("Default print");
}

public class Report : IPrintable
{
    public void Print() => Console.WriteLine("Printing report...");
}

public class Invoice : IPrintable
{
    public void Print() => Console.WriteLine("Printing invoice...");
}

// Multiple capabilities:
public interface IExportable { void ExportCsv(string path); }

public class Ledger : IPrintable, IExportable
{
    public void Print() => Console.WriteLine("Printing ledger...");
    public void ExportCsv(string path) => File.WriteAllText(path, "header,rows...");
}
```

## Q4. What’s the difference between `virtual`, `override`, and `new` in C#?

**Question:**  
What do the C# keywords `virtual`, `override`, and `new` mean, when should each be used, and how do they affect polymorphism?

**Answer:**  
At a glance:

| Keyword   | Declared In       | Purpose                                    | Polymorphism |
|-----------|-------------------|--------------------------------------------|--------------|
| `virtual` | Base class        | Marks a member as overridable              | ✅ Yes       |
| `override`| Derived class     | Replaces a `virtual`/`abstract`/`override` member from base | ✅ Yes |
| `new`     | Derived class     | Hides a base member with the **same name/signature** (method hiding) | ❌ No (static dispatch) |

**Key points:**
- `override` **requires** a matching `virtual`/`abstract`/`override` member in the base with the **same signature** and **compatible return type** (covariant returns allowed).
- `new` performs **member hiding** (compile-time). Dispatch depends on the **static type** of the variable, not the runtime type.
- You can combine `new` with `virtual` to start a **new virtual slot** in the derived class: `public new virtual void M() { ... }`.
- Use `sealed override` to stop further overrides.
- Non-virtual base members **cannot** be overridden—only hidden with `new`.

**Example (override vs new):**
```csharp
public class Base
{
    public virtual void Speak() => Console.WriteLine("Base.Speak (virtual)");
    public void Ping() => Console.WriteLine("Base.Ping (non-virtual)");
}

public class OverrideChild : Base
{
    public override void Speak() => Console.WriteLine("OverrideChild.Speak (override)");
    // Hides the non-virtual method; static dispatch decides
    public new void Ping() => Console.WriteLine("OverrideChild.Ping (new/hide)");
}

public class NewChild : Base
{
    // Hides the virtual method; no polymorphism for Speak via Base reference
    public new void Speak() => Console.WriteLine("NewChild.Speak (new/hide)");
}

Base b1 = new OverrideChild();
Base b2 = new NewChild();
OverrideChild o = new OverrideChild();

b1.Speak(); // OverrideChild.Speak (override) -> polymorphic (runtime)
b2.Speak(); // Base.Speak (virtual) -> because NewChild hid, not override
b1.Ping();  // Base.Ping (non-virtual) -> static type Base
o.Ping();   // OverrideChild.Ping (new/hide) -> static type OverrideChild
```

## Q5. What’s the difference between **overload** and **override** in C#?

**Question:**  
Explain the difference between **method overloading** and **method overriding** in C#, with simple examples.

**Answer:**  
**Summary**

| Concept     | overload                                 | override                                   |
|-------------|-------------------------------------------|--------------------------------------------|
| Meaning     | Same method name, **different signature** | Child class **replaces** parent’s method   |
| When        | **Compile time** resolution               | **Run time** (virtual dispatch)            |
| Where       | **Same class** (or within the type)       | **Between base and derived** classes       |

> Overload = different parameters.  
> Override = `virtual` in base + `override` in derived.

**Example — Overload (same class):**
```csharp
public class MathUtil
{
    public int Sum(int a, int b) => a + b;                // Sum(int,int)
    public int Sum(int a, int b, int c) => a + b + c;     // Sum(int,int,int)
    public double Sum(double a, double b) => a + b;       // Sum(double,double)
}

// Compile-time picks which Sum(...) based on the argument types/count.
```
**Example — Override (base vs derived):**
```csharp
public class Greeter
{
    public virtual void SayHello() => Console.WriteLine("Hello from base");
}

public class LoudGreeter : Greeter
{
    public override void SayHello() => Console.WriteLine("HELLO from child!");
}

Greeter g = new LoudGreeter();
g.SayHello(); // Run-time calls LoudGreeter.SayHello (override)
```

## Q6. What’s the difference between **protected** and **private** in C#?

**Question:**  
Explain the difference between the access modifiers **protected** and **private** in C#, with simple examples.

**Answer:**  
- **private** → Member is only accessible **inside the same class**.  
- **protected** → Member is accessible **inside the same class and in derived classes**.  

**Example — private (only inside class):**
```csharp
public class Car
{
    private int speed = 0;

    public void Accelerate()
    {
        speed += 10; // allowed, inside the same class
    }
}

public class SportsCar : Car
{
    public void Test()
    {
        // speed = 100; ❌ not accessible, because it's private in Car
    }
}
```
```csharp
public class Car
{
    protected int speed = 0;

    public void Accelerate()
    {
        speed += 10; // allowed
    }
}

public class SportsCar : Car
{
    public void Boost()
    {
        speed += 50; // ✅ accessible, because it's protected
    }
}

```