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

## Q2. چهار اصل اصلی OOP چیست؟

**Question:**  
چهار اصل اصلی برنامه‌نویسی شی‌ءگرا (OOP) چیست؟

**Answer:**  
چهار اصل اصلی OOP عبارت‌اند از **Encapsulation (کپسوله‌سازی)**، **Inheritance (وراثت)**، **Polymorphism (چندریختی)** و **Abstraction (انتزاع)**. در ادامه، تعریف دقیق هرکدام به‌همراه مثال‌های متداول #C آمده است.

### 1. Encapsulation (کپسوله‌سازی)  
کپسوله‌سازی یعنی **پنهان کردن وضعیت داخلی شیء** و در معرض قرار دادن فقط آنچه لازم است از طریق یک **رابط کنترل‌شده** (Property/Method).  
- با **سطوح دسترسی** (`private`, `protected`, `internal`, `public`) محقق می‌شود.  
- به‌جای فیلدهای عمومی از **Property**‌ها (همراه با اعتبارسنجی) استفاده کنید.  
- به حفظ **قیود ثابت (Invariants)** کمک می‌کند و مانع استفادهٔ نادرست یا ناخواسته می‌شود.

**Example:**
```csharp
public class BankAccount
{
    private decimal balance; // از بیرون پنهان

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

### ۲. Inheritance (وراثت)  
وراثت به یک **کلاسِ مشتق** اجازه می‌دهد رفتارها و اعضای **کلاسِ پایه** را **بازاستفاده** کرده و در صورت نیاز **گسترش** دهد.  
- در C#، کلاس‌ها فقط **تک‌وراثتی** هستند، اما می‌توان **چندین اینترفیس** را هم‌زمان پیاده‌سازی کرد.  
- برای سفارشی‌سازی رفتار از `virtual`/`override` استفاده کنید؛ و برای جلوگیری از overrideهای بیشتر می‌توانید از `sealed` بهره ببرید.

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

### 3. Polymorphism (چندریختی)  
چندریختی یعنی **یک عملیات واحد** بسته به **نوع واقعی شیء**، **رفتار متفاوتی** نشان بدهد.  
- در زمان اجرا از طریق `virtual` در کلاس پایه و `override` در کلاس مشتق پیاده‌سازی می‌شود.  
- این رویکرد امکان **طراحی مبتنی بر انتزاعات** (کلاس‌های پایه/اینترفیس‌ها) را به‌جای وابستگی به پیاده‌سازی‌های خاص فراهم می‌کند.

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

### ۴. Abstraction (انتزاع)  
انتزاع روی این تمرکز دارد که یک شیء **چه کاری انجام می‌دهد**، نه این‌که **چطور** آن را انجام می‌دهد.  
- قراردادها را با **interface**‌ها یا **abstract class**‌ها تعریف می‌کنیم.  
- جزئیات پیاده‌سازی پنهان می‌ماند و فقط عملیات ضروری در معرض دید قرار می‌گیرد.

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

## Q3. تفاوت‌های کلیدی بین abstract class و interface در C# چیست؟

**Question:**  
تفاوت‌های اصلی بین **abstract class** و **interface** در C# چیست و هرکدام را چه زمانی استفاده کنیم؟

**Answer:**  
خلاصهٔ تفاوت‌ها:

| ویژگی                                   | abstract class                                  | interface                                                                 |
|-----------------------------------------|--------------------------------------------------|---------------------------------------------------------------------------|
| امکان داشتن پیاده‌سازی                   | ✅ بله (اعضای abstract **و** غیر abstract)        | ⚠️ به‌صورت سنتی **خیر**؛ از **C# 8** به بعد می‌تواند **default implementation** داشته باشد |
| امکان داشتن فیلد (state)                | ✅ بله (فیلدهای نمونه)                           | ❌ خیر (فیلد نمونه ندارد)                                                |
| داشتن سازنده (constructor)              | ✅ بله                                           | ❌ خیر (سازندهٔ نمونه ندارد)                                             |
| تعداد ارث‌بری/پیاده‌سازی                | ✅ فقط یک کلاس پایه                              | ✅ یک کلاس می‌تواند **چندین** interface را پیاده‌سازی کند                |
| سطح دسترسی اعضا                         | ✅ متنوع (`public/protected/internal/...`)       | ⚠️ قرارداد عمومی؛ اعضا عملاً بخشی از سطح عمومی نوع هستند                 |
| سناریوی استفاده                         | اشتراک **state + رفتار پایهٔ مشترک**            | تعریف **قرارداد/قابلیت** بین انواع نامرتبط                              |

**راهنما:**  
- وقتی **state** مشترک، **توابع محافظت‌شده** و **بخشی از پیاده‌سازی مشترک** لازم دارید، از **abstract class** استفاده کنید.  
- وقتی می‌خواهید یک **قابلیت/قرارداد** را برای انواع مختلف (حتی نامرتبط) تعریف کنید، از **interface** استفاده کنید.  
- از **C# 8+**، امکان **default method** در interface وجود دارد؛ ولی همچنان **فیلد نمونه** ندارد و نقش اصلی آن **قرارداد** است.

**نمونه‌ها**

*abstract class (state و رفتار مشترک):*
```csharp
public abstract class Shape
{
    // وضعیت مشترک
    protected double _x, _y;

    protected Shape(double x, double y)
    {
        _x = x; _y = y;
    }

    // باید در فرزند پیاده‌سازی شود
    public abstract double Area();

    // رفتار مشترک اختیاری
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
    // C# 8+: امکان پیاده‌سازی پیش‌فرض
    // void Print() => Console.WriteLine("چاپ پیش‌فرض");
}

public class Report : IPrintable
{
    public void Print() => Console.WriteLine("در حال چاپ گزارش...");
}

public class Invoice : IPrintable
{
    public void Print() => Console.WriteLine("در حال چاپ فاکتور...");
}

// قابلیت‌های چندگانه:
public interface IExportable { void ExportCsv(string path); }

public class Ledger : IPrintable, IExportable
{
    public void Print() => Console.WriteLine("در حال چاپ دفتر...");
    public void ExportCsv(string path) => File.WriteAllText(path, "header,rows...");
}

```

## Q4. تفاوت `virtual`، `override` و `new` در C# چیست؟

**Question:**  
کلیدواژه‌های `virtual`، `override` و `new` در C# چه معنی‌ای دارند، هرکدام چه زمانی استفاده می‌شوند و چه اثری روی چندریختی (polymorphism) دارند؟

**Answer:**  
خلاصهٔ تفاوت‌ها:

| کلیدواژه  | محل اعلان         | کاربرد                                     | چندریختی |
|-----------|--------------------|--------------------------------------------|----------|
| `virtual` | کلاس پایه          | قابلِ override کردن می‌کند                 | ✅ بله    |
| `override`| کلاس مشتق          | جایگزینِ عضوِ `virtual`/`abstract`/`override` در پایه | ✅ بله |
| `new`     | کلاس مشتق          | **مخفی‌سازی** عضوِ همنام/هم‌امضا در پایه (method hiding) | ❌ خیر (ارسال ایستا) |

**نکات کلیدی:**
- `override` فقط وقتی مجاز است که در کلاس پایه عضوی با **امضای یکسان** و یکی از حالت‌های `virtual`/`abstract`/`override` وجود داشته باشد (بازگشت هم‌خوان؛ **covariant return** مجاز).
- `new` فقط **مخفی‌سازی** انجام می‌دهد و ارسال (dispatch) به‌صورت **ایستا** و بر اساس **نوعِ ایستای** متغیر انجام می‌شود، نه نوع زمان اجرا.
- می‌توان `new` را با `virtual` ترکیب کرد تا یک **شکاف مجازی جدید** در کلاس مشتق بسازید: `public new virtual void M() { ... }`.
- با `sealed override` می‌توانید جلوی overrideهای بعدی را بگیرید.
- اعضای غیرمجازی در پایه **قابل override نیستند**؛ فقط می‌توان آن‌ها را با `new` مخفی کرد.

**مثال (تفاوت override و new):**
```csharp
public class Base
{
    public virtual void Speak() => Console.WriteLine("Base.Speak (virtual)");
    public void Ping() => Console.WriteLine("Base.Ping (non-virtual)");
}

public class OverrideChild : Base
{
    public override void Speak() => Console.WriteLine("OverrideChild.Speak (override)");
    // مخفی‌سازی عضو غیرمجازی؛ ارسال ایستا تعیین‌کننده است
    public new void Ping() => Console.WriteLine("OverrideChild.Ping (new/hide)");
}

public class NewChild : Base
{
    // مخفی‌سازی عضو مجازی؛ از دید مرجعِ Base چندریختی رخ نمی‌دهد
    public new void Speak() => Console.WriteLine("NewChild.Speak (new/hide)");
}

Base b1 = new OverrideChild();
Base b2 = new NewChild();
OverrideChild o = new OverrideChild();

b1.Speak(); // OverrideChild.Speak (override) -> چندریختی زمان اجرا
b2.Speak(); // Base.Speak (virtual) -> چون NewChild override نکرده، فقط hide کرده است
b1.Ping();  // Base.Ping (non-virtual) -> نوع ایستای متغیر Base است
o.Ping();   // OverrideChild.Ping (new/hide) -> نوع ایستای متغیر OverrideChild است
```

## Q5. تفاوت **overload** و **override** در C# چیست؟

**Question:**  
تفاوت **overload** (هم‌نامی با امضای متفاوت) و **override** (بازنویسی متد والد) را با مثال‌های ساده توضیح دهید.

**Answer:**  
**خلاصه**

| مفهوم       | overload                                   | override                                  |
|-------------|--------------------------------------------|-------------------------------------------|
| یعنی چی؟    | نام یکسان، **امضای متفاوت**                | کلاس فرزند **متد والد را بازنویسی** می‌کند |
| زمان اجرا؟  | انتخاب در **Compile time**                 | انتخاب در **Run time** (ارسال مجازی)      |
| کجا؟        | در **همان کلاس**                           | بین **کلاس پایه و فرزند**                 |

> Overload = پارامترها فرق می‌کنند.  
> Override = متد پایه `virtual` و در فرزند `override`.

**مثال — Overload (در همان کلاس):**
```csharp
public class MathUtil
{
    public int Sum(int a, int b) => a + b;                // Sum(int,int)
    public int Sum(int a, int b, int c) => a + b + c;     // Sum(int,int,int)
    public double Sum(double a, double b) => a + b;       // Sum(double,double)
}

// انتخاب نسخهٔ درستِ Sum(...) در زمان کامپایل بر اساس نوع/تعداد آرگومان‌ها انجام می‌شود.
```
**مثال — Override (بین پایه و فرزند):**
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
g.SayHello(); // در زمان اجرا متد فرزند صدا زده می‌شود (override)
```

## Q6. تفاوت **protected** و **private** در C# چیست؟

**Question:**  
تفاوت سطح دسترسی **protected** و **private** در C# را با مثال ساده توضیح دهید.

**Answer:**  
- **private** → فقط در **خود کلاس** قابل دسترسی است.  
- **protected** → در **خود کلاس و کلاس‌های فرزند** قابل دسترسی است.  

**مثال — private (فقط داخل همان کلاس):**
```csharp
public class Car
{
    private int speed = 0;

    public void Accelerate()
    {
        speed += 10; // مجاز، داخل همان کلاس
    }
}

public class SportsCar : Car
{
    public void Test()
    {
        // speed = 100; ❌ دسترسی غیرممکن، چون private در Car است
    }
}
```
```csharp
public class Car
{
    protected int speed = 0;

    public void Accelerate()
    {
        speed += 10; // مجاز
    }
}

public class SportsCar : Car
{
    public void Boost()
    {
        speed += 50; // ✅ مجاز، چون protected است
    }
}
```

## Q7. تفاوت **Composition** و **Inheritance** در OOP چیست؟

**Question:**  
تفاوت **ترکیب (Composition)** و **وراثت (Inheritance)** در برنامه‌نویسی شی‌ءگرا چیست؟ چه زمانی باید از هرکدام استفاده کرد؟ با مثال ساده در #C توضیح دهید.

**Answer:**  
- **Inheritance (وراثت)** → روشی که در آن یک کلاس از کلاس دیگری ارث‌بری می‌کند تا اعضا و رفتار آن را استفاده کرده یا بازنویسی کند. (رابطهٔ "است").  
- **Composition (ترکیب)** → روشی که در آن یک کلاس شیءهای کلاس‌های دیگر را در خود نگه می‌دارد و از قابلیت‌های آن‌ها استفاده می‌کند. (رابطهٔ "دارد").  

**مقایسه**

| جنبه          | Inheritance ("است")                   | Composition ("دارد")                       |
|---------------|---------------------------------------|--------------------------------------------|
| رابطه         | Dog **یک Animal است**                 | Car **یک Engine دارد**                     |
| میزان وابستگی | بالا (کلاس فرزند به طراحی والد وابسته)| پایین‌تر (انعطاف‌پذیر، اجزاء قابل‌تعویض)  |
| استفاده مجدد  | با گسترش کلاس والد                    | با واگذاری رفتار به شیءهای داخلی            |
| انعطاف‌پذیری  | تغییر سلسله‌مراتب دشوار              | تغییر/جایگزینی اجزاء آسان‌تر               |

**مثال — وراثت:**
```csharp
public class Animal
{
    public virtual void Speak() => Console.WriteLine("Some sound");
}

public class Dog : Animal
{
    public override void Speak() => Console.WriteLine("Bark");
}

Animal a = new Dog();
a.Speak(); // Bark
```
```csharp
// مثال — ترکیب:
public class Engine
{
    public void Start() => Console.WriteLine("Engine starting...");
}

public class Car
{
    private Engine _engine = new Engine(); // یک Engine دارد

    public void Start()
    {
        _engine.Start(); // واگذاری رفتار
        Console.WriteLine("Car is ready.");
    }
}

Car c = new Car();
c.Start();
// خروجی:
// Engine starting...
// Car is ready.
```
- وقتی رابطهٔ "است" (is-a) واضح است و می‌خواهید از چندریختی استفاده کنید، از وراثت بهره ببرید.
- وقتی انعطاف، وابستگی کمتر و تست‌پذیری مهم‌تر است، ترکیب انتخاب بهتری است. (اصل معروف: "Composition over Inheritance").