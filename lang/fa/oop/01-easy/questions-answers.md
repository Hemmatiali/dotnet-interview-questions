## Q1. OOP یعنی چی؟

**Question:**  
OOP یعنی چی؟

**Answer:**  
OOP یا همان **برنامه‌نویسی شی‌ءگرا** یک پارادایم برنامه‌نویسی است که در آن کدها به صورت **شی‌ء (Object)** ساخته و سازمان‌دهی می‌شوند. هر شی‌ء از دو بخش اصلی تشکیل شده است:

1. **داده (Data / Property / Field)** – اطلاعاتی که آن شی‌ء را توصیف می‌کند.  
2. **رفتار (Behavior / Method / Function)** – عملیاتی که آن شی‌ء می‌تواند انجام دهد.

ترکیب داده و رفتار در یک واحد باعث می‌شود بتوانیم موجودیت‌های دنیای واقعی را به شکل طبیعی‌تری در نرم‌افزار مدل‌سازی کنیم.

مثال ساده در #C:

```csharp
public class Car
{
    // داده (state)
    public string Color { get; set; }
    public int Speed { get; set; }

    // رفتار (methods)
    public void Drive()
    {
        Console.WriteLine("ماشین در حال حرکت است...");
    }

    public void Stop()
    {
        Console.WriteLine("ماشین متوقف شد.");
    }
}
```