## Q1. What is the difference between **compile time** and **run time** in C#?

**Question:**  
Explain what **compile time** and **run time** mean in C#, and compare them with simple examples.

**Answer:**  
- **Compile time** → The phase when source code is checked and translated into IL (Intermediate Language). Errors like syntax mistakes or wrong method calls are caught here.  
- **Run time** → The phase when the program is actually executed by the CLR. Errors like dividing by zero or null reference exceptions happen here.  

**Comparison**

| Aspect          | Compile Time                          | Run Time                           |
|-----------------|---------------------------------------|-------------------------------------|
| Checked by      | Compiler (C# → IL)                    | CLR (when executing code)           |
| Typical errors  | Syntax error, type mismatch           | NullReferenceException, DivideByZero|
| When            | Before execution (build)              | During program execution            |

**Example — Compile-time error:**
```csharp
public class Demo
{
    public void Test()
    {
        int x = "hello"; // ❌ compile-time error (type mismatch)
    }
}
```
```csharp
public class Demo
{
    public void Test()
    {
        int a = 10, b = 0;
        int result = a / b; // ❌ run-time error (DivideByZeroException)
    }
}
```