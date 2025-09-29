## Q1. تفاوت **Compile Time** و **Run Time** در C# چیست؟

**Question:**  
منظور از **Compile Time** و **Run Time** در C# چیست؟ آن‌ها را مقایسه کنید و با مثال توضیح دهید.

**Answer:**  
- **Compile Time (زمان کامپایل)** → زمانی که کد منبع بررسی و به IL ترجمه می‌شود. خطاهای نحوی یا ناسازگاری نوع در این مرحله کشف می‌شوند.  
- **Run Time (زمان اجرا)** → زمانی که برنامه واقعاً توسط CLR اجرا می‌شود. خطاهایی مثل تقسیم بر صفر یا NullReferenceException در این مرحله رخ می‌دهند.  

**مقایسه**

| جنبه             | Compile Time                      | Run Time                              |
|------------------|-----------------------------------|---------------------------------------|
| بررسی توسط       | کامپایلر (C# → IL)                | CLR هنگام اجرای برنامه                 |
| خطاهای معمول     | خطای نحوی، ناسازگاری نوع          | NullReferenceException، DivideByZero  |
| زمان             | قبل از اجرا (ساخت برنامه)         | حین اجرای برنامه                      |

**مثال — خطای Compile-time:**
```csharp
public class Demo
{
    public void Test()
    {
        int x = "hello"; // ❌ خطای کامپایل (ناسازگاری نوع)
    }
}
```
```csharp
public class Demo
{
    public void Test()
    {
        int a = 10, b = 0;
        int result = a / b; // ❌ خطای زمان اجرا (DivideByZeroException)
    }
}
```

## Q2. IL چیست و کامپایلر C# چگونه کار می‌کند؟

**Question:**  
IL در .NET چیست و کامپایلر C# چطور کد را به اجرا درمی‌آورد؟

**Answer:**  
- **IL (Intermediate Language)** یا **MSIL / CIL** یک زبان میانی سطح پایین و مستقل از CPU است که خروجی کامپایلر C# می‌باشد.  
- **کامپایلر C# (csc.exe)** کد منبع را به **IL + متادیتا** تبدیل کرده و در فایل اسمبلی (.dll یا .exe) ذخیره می‌کند.  
- در زمان اجرا، **JIT Compiler** در CLR، کد IL را به **دستورات ماشین واقعی** مخصوص CPU و سیستم‌عامل تبدیل کرده و اجرا می‌کند.  

**مراحل کلی کامپایل در .NET:**
1. کد #C در فایل‌های `.cs` نوشته می‌شود.  
2. کامپایلر آن را به **IL + metadata** تبدیل می‌کند و در اسمبلی ذخیره می‌شود.  
3. **JIT Compiler** در زمان اجرا IL را به کد بومی ترجمه می‌کند.  
4. CLR مدیریت اجرا، حافظه، GC و امنیت را بر عهده دارد.  

**مثال (کد C# → IL):**
```csharp
public class Demo
{
    public int Add(int a, int b) => a + b;
}
```
```csharp
// معادل IL (خلاصه‌شده، با ابزار ILDASM/ILSpy قابل مشاهده):
.class public auto ansi beforefieldinit Demo extends [System.Runtime]System.Object
{
    .method public hidebysig instance int32 Add(int32 a, int32 b) cil managed
    {
        .maxstack 2
        ldarg.1
        ldarg.2
        add
        ret
    }
}
```