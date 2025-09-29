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