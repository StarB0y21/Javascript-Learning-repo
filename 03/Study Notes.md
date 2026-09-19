## Created by AI


# جزوه آموزشی: JavaScript Engine، اجرای کد و مدیریت حافظه

> این جزوه مسیر اجرای JavaScript از دریافت کد تا اجرای آن روی CPU و همچنین رابطه‌ی Stack، Heap و Garbage Collection را مرحله‌به‌مرحله توضیح می‌دهد.
>
> **نکته:** بعضی از واژه‌های فهرست اولیه، مثل `Byte Stream Decoder` یا `Binary`، در موتورهای واقعی ممکن است با نام‌ها و مراحل دقیقاً یکسانی پیاده‌سازی نشده باشند. در این جزوه، مفهوم عمومی آن‌ها را توضیح می‌دهیم و در جاهایی که لازم است مدل را دقیق‌تر می‌کنیم.

---

# بخش اول — JavaScript Engine چیست؟

## 1. JavaScript Engine

**JavaScript Engine** برنامه‌ای است که کد JavaScript را دریافت می‌کند، آن را تحلیل و اجرا می‌کند.

مثلاً مرورگر کد زیر را دریافت می‌کند:

```js
const x = 10;
const y = 20;
console.log(x + y);
```

موتور JavaScript باید بفهمد:

1. `const` چیست؟
2. `x` و `y` چه متغیرهایی هستند؟
3. `10` و `20` چه مقادیری هستند؟
4. `x + y` چه عملیاتی انجام می‌دهد؟
5. در نهایت چه دستوراتی باید روی CPU اجرا شوند؟

نمونه‌هایی از JavaScript Engineها:

- **V8** → Chrome و Node.js
- **SpiderMonkey** → Firefox
- **JavaScriptCore** → Safari

---

# بخش دوم — تصویر کلی Pipeline

یک مدل ساده‌شده از مسیر اجرای JavaScript:

```text
JavaScript Source Code
        ↓
Byte Stream
        ↓
Byte Stream Decoder
        ↓
Parser
        ↓
AST
        ↓
Bytecode / Interpreter
        ↓
Profiler / Type Feedback
        ↓
Optimizing Compiler
        ↓
Machine Code
        ↓
CPU Execution
```

اما باید یک نکته مهم را بدانیم:

> موتورهای مدرن JavaScript معمولاً صرفاً «Interpreter یا Compiler» نیستند؛ از ترکیب Interpreter، Profiler و چند سطح Compiler استفاده می‌کنند.

در V8 مثلاً مسیر مفهومی مدرن‌تر چیزی شبیه این است:

```text
Source Code
    ↓
Parser
    ↓
AST
    ↓
Bytecode
    ↓
Interpreter
    ↓
Profiling / Type Feedback
    ↓
Optimizing Compiler
    ↓
Optimized Machine Code
```

---

# بخش سوم — Network، Cache و Service Worker

قبل از اینکه JavaScript توسط Engine اجرا شود، مرورگر باید فایل JavaScript را به دست بیاورد.

فرض کنید HTML شامل این باشد:

```html
<script src="app.js"></script>
```

مرورگر باید `app.js` را پیدا و دریافت کند.

## 3.1 Network

اگر فایل در Cache موجود نباشد، مرورگر ممکن است آن را از شبکه دریافت کند:

```text
Browser
   ↓
HTTP Request
   ↓
Server
   ↓
app.js
   ↓
Browser
```

مثلاً:

```http
GET /app.js HTTP/1.1
```

و سرور چیزی شبیه این برمی‌گرداند:

```js
console.log("Hello");
```

---

## 3.2 Cache

مرورگر می‌تواند منابعی مثل JavaScript، CSS و تصاویر را Cache کند.

هدف:

```text
First request:
Browser → Network → Server

Later request:
Browser → Cache
```

در نتیجه لازم نیست همیشه فایل را از شبکه دریافت کند.

مثلاً اگر `app.js` قبلاً دریافت شده باشد، مرورگر ممکن است نسخه Cache‌شده را استفاده کند.

### چرا Cache مهم است؟

چون:

- مصرف شبکه کمتر می‌شود.
- سرعت بارگذاری افزایش پیدا می‌کند.
- تعداد درخواست‌ها به Server کاهش پیدا می‌کند.

---

# بخش چهارم — Service Worker

**Service Worker** یک JavaScript worker است که می‌تواند بین مرورگر و Network قرار بگیرد.

مدل ساده:

```text
              ┌──────────────┐
              │    Browser   │
              └──────┬───────┘
                     ↓
              Service Worker
                ↙         ↘
             Cache       Network
```

مثلاً Service Worker می‌تواند درخواست فایل را بگیرد:

```js
self.addEventListener("fetch", event => {
    // بررسی درخواست
});
```

و تصمیم بگیرد فایل از Cache خوانده شود یا Network.

این موضوع در PWAها بسیار مهم است.

---

# بخش پنجم — Byte Stream

وقتی فایل JavaScript دریافت می‌شود، چیزی که واقعاً از Network می‌آید «متن مفهومی» نیست؛ داده به شکل **Byte** منتقل می‌شود.

مثلاً:

```js
const x = 10;
```

در سطح پایین‌تر به مجموعه‌ای از Byteها تبدیل می‌شود.

نمایش ساده:

```text
01101010 01110011 00100000 ...
```

این داده را می‌توان به عنوان یک **Byte Stream** در نظر گرفت.

### Byte چیست؟

یک Byte برابر 8 بیت است:

```text
1 Byte = 8 bits
```

مثلاً:

```text
01000001
```

در ASCII نماینده‌ی حرف `A` است.

---

# بخش ششم — Byte Stream Decoder

مرورگر باید Byteها را مطابق Encoding به کاراکترها تبدیل کند.

رایج‌ترین Encoding برای Web:

```text
UTF-8
```

مثلاً متن:

```js
const name = "Sina";
```

به شکل Byte منتقل می‌شود و Decoder آن را به کاراکترهای قابل پردازش تبدیل می‌کند.

مدل ساده:

```text
Bytes
  ↓
UTF-8 Decoder
  ↓
Characters
  ↓
Source Text
```

### نکته مهم

`Byte Stream Decoder` را نباید با Parser یکی بدانیم.

Decoder می‌گوید:

> این Byteها چه کاراکترهایی هستند؟

Parser می‌گوید:

> این کاراکترها از نظر Grammar زبان JavaScript چه معنایی دارند؟

---

# بخش هفتم — Parser

**Parser** بخشی از Engine است که Source Code را بر اساس قواعد زبان JavaScript تحلیل می‌کند.

مثلاً:

```js
const x = 10;
```

Parser تشخیص می‌دهد که:

```text
const
```

یک Keyword است.

```text
x
```

یک Identifier است.

```text
10
```

یک Numeric Literal است.

و کل عبارت یک Variable Declaration محسوب می‌شود.

---

# بخش هشتم — Syntax

Parser بر اساس **Syntax** زبان کار می‌کند.

مثلاً:

```js
const x = 10;
```

صحیح است.

اما:

```js
const = x 10;
```

از نظر Syntax مشکل دارد.

در این حالت Parser نمی‌تواند ساختار مورد انتظار را تشکیل دهد و Syntax Error رخ می‌دهد.

مثلاً:

```js
const x =
```

باعث خطایی مشابه این می‌شود:

```text
SyntaxError: Unexpected end of input
```

---

# بخش نهم — AST

AST مخفف:

```text
Abstract Syntax Tree
```

یعنی:

> درخت نحوی انتزاعی

Parser پس از تحلیل کد، ساختاری درختی تولید می‌کند.

مثلاً:

```js
const x = 10;
```

به صورت مفهومی:

```text
VariableDeclaration
│
├── Identifier: x
│
└── Literal: 10
```

---

# بخش دهم — چرا AST به شکل Tree است؟

چون برنامه ساختار تو در تو دارد.

مثلاً:

```js
const result = a + b;
```

ساختار مفهومی:

```text
VariableDeclaration
│
├── Identifier: result
│
└── BinaryExpression (+)
    │
    ├── Identifier: a
    └── Identifier: b
```

می‌توانیم این را مثل یک درخت تصور کنیم:

```text
          =
        /   \
   result     +
             / \
            a   b
```

---

# بخش یازدهم — AST چه کاربردی دارد؟

AST فقط برای اجرای JavaScript نیست.

ابزارهای زیادی از AST استفاده می‌کنند:

- Babel
- ESLint
- Prettier
- TypeScript tooling
- Minifiers
- Code analyzers

مثلاً ESLint می‌تواند کد را بررسی کند و تشخیص دهد که یک متغیر استفاده نشده است.

---

# بخش دوازدهم — Bytecode

بعد از تحلیل Source Code، موتور می‌تواند آن را به **Bytecode** تبدیل کند.

Bytecode کدی میانی است که توسط Interpreter موتور اجرا می‌شود.

مثلاً به صورت بسیار ساده:

```text
Source:
const x = 10;
console.log(x);
```

ممکن است به دستورهای داخلی مشابه این تبدیل شود:

```text
CreateConstant 10
StoreVariable x
LoadVariable x
Call console.log
```

این فقط مدل آموزشی است و Bytecode واقعی V8 دقیقاً به این شکل نیست.

---

# بخش سیزدهم — Interpreter

**Interpreter** دستورهای Bytecode را اجرا می‌کند.

مدل ساده:

```text
JavaScript
    ↓
AST
    ↓
Bytecode
    ↓
Interpreter
    ↓
Execution
```

فرض کنید:

```js
const x = 10;
console.log(x);
```

Interpreter دستورهای داخلی مربوط به ساخت مقدار، ذخیره متغیر و فراخوانی تابع را اجرا می‌کند.

---

# بخش چهاردهم — چرا فقط Interpreter کافی نیست؟

اگر موتور مجبور باشد همه چیز را همیشه از طریق Interpreter اجرا کند، بعضی کدهای پرتکرار می‌توانند فرصت بهینه‌سازی داشته باشند.

مثلاً:

```js
function add(a, b) {
    return a + b;
}

for (let i = 0; i < 1000000; i++) {
    add(i, 10);
}
```

تابع `add` بارها اجرا می‌شود.

Engine می‌تواند متوجه شود:

> این تابع دائماً با مقادیر عددی استفاده می‌شود.

اینجا **Profiling** و **Type Feedback** اهمیت پیدا می‌کنند.

---

# بخش پانزدهم — Profiler

Profiler اطلاعاتی درباره رفتار واقعی برنامه جمع می‌کند.

مثلاً:

```js
function square(x) {
    return x * x;
}
```

اگر برنامه بارها این کار را انجام دهد:

```js
square(2);
square(5);
square(10);
square(20);
```

Engine متوجه می‌شود که `x` معمولاً Number است.

این اطلاعات می‌تواند برای Optimization استفاده شود.

---

# بخش شانزدهم — Type Feedback

**Type Feedback** یعنی Engine از اجرای واقعی برنامه اطلاعاتی درباره Typeها به دست می‌آورد.

مثلاً:

```js
function add(a, b) {
    return a + b;
}
```

اگر بیشتر فراخوانی‌ها این‌گونه باشند:

```js
add(10, 20);
add(30, 40);
add(100, 200);
```

Engine می‌تواند مشاهده کند:

```text
a → Number
b → Number
```

اما اگر بعداً بنویسیم:

```js
add("Hello ", "World");
```

رفتار متفاوت می‌شود، چون `+` برای String معنای دیگری دارد.

---

# بخش هفدهم — Optimizing Compiler

**Optimizing Compiler** قسمت‌هایی از برنامه را که زیاد اجرا می‌شوند و اطلاعات کافی درباره آن‌ها وجود دارد، به شکل بهینه‌تری Compile می‌کند.

مدل:

```text
Bytecode
   ↓
Interpreter
   ↓
Profiler / Type Feedback
   ↓
Hot Code
   ↓
Optimizing Compiler
   ↓
Machine Code
```

---

# بخش هجدهم — Hot Code

بخش‌هایی از برنامه که بسیار زیاد اجرا می‌شوند، معمولاً به عنوان **Hot Code** شناخته می‌شوند.

مثلاً:

```js
function sum(a, b) {
    return a + b;
}

for (let i = 0; i < 10000000; i++) {
    sum(i, 1);
}
```

تابع `sum` بارها اجرا می‌شود.

Engine ممکن است تصمیم بگیرد این بخش ارزش Optimization دارد.

---

# بخش نوزدهم — Machine Code

CPU مستقیماً JavaScript یا معمولاً Bytecode موتور JavaScript را اجرا نمی‌کند.

CPU در نهایت دستورهای Machine Code مربوط به معماری پردازنده را اجرا می‌کند.

مثلاً روی CPUهای مختلف، Instruction Set می‌تواند متفاوت باشد:

```text
x86-64
ARM64
...
```

پس:

```text
JavaScript
   ↓
Engine
   ↓
Machine Code
   ↓
CPU
```

---

# بخش بیستم — Binary

کلمه‌ی **Binary** در این بحث می‌تواند گمراه‌کننده باشد.

تمام داده‌های کامپیوتری در نهایت به شکل بیت‌ها نمایش داده می‌شوند، اما در مسیر اجرای JavaScript بهتر است دقیق‌تر بگوییم:

```text
Source Code
→ Bytecode
→ Machine Code
→ CPU
```

Machine Code خود مجموعه‌ای از دستورهای باینری برای پردازنده است.

---

# بخش بیست‌ویکم — JIT Compilation

JIT مخفف:

```text
Just-In-Time Compilation
```

یعنی Compile کردن کد در زمان اجرای برنامه.

JavaScript مدرن معمولاً از ترکیبی از:

```text
Interpretation
+
Profiling
+
JIT Compilation
+
Optimization
```

استفاده می‌کند.

بنابراین این مدل:

```text
JS → Compiler → Machine Code
```

برای آموزش ابتدایی مفید است، اما تصویر کاملی از Engine مدرن نیست.

---

# بخش بیست‌ودوم — Deoptimization

Optimization همیشه بدون ریسک نیست.

فرض کنیم Engine تشخیص دهد:

```js
function multiply(a, b) {
    return a * b;
}
```

تقریباً همیشه با Number استفاده می‌شود.

ممکن است نسخه‌ای بهینه تولید شود.

اما اگر شرایطی پیش بیاید که فرض Optimization دیگر معتبر نباشد، Engine می‌تواند **Deoptimize** کند.

یعنی از مسیر بهینه خارج شود و به اجرای عمومی‌تر برگردد.

مدل:

```text
Bytecode
   ↓
Optimization
   ↓
Optimized Machine Code
   ↓
Assumption breaks
   ↓
Deoptimization
   ↓
Generic execution
```

---

# بخش بیست‌وسوم — Memory

برای اجرای برنامه، Engine باید داده‌های برنامه را در حافظه نگهداری کند.

دو مفهوم مهم:

```text
Stack
Heap
```

---

# بخش بیست‌وچهارم — Call Stack

**Call Stack** ساختاری برای مدیریت اجرای Functionها است.

مثلاً:

```js
function first() {
    second();
}

function second() {
    third();
}

function third() {
    console.log("Hello");
}

first();
```

هنگام اجرا:

```text
third()
second()
first()
global
```

از بالا به پایین Stack Frameها مدیریت می‌شوند.

---

# بخش بیست‌وپنجم — Stack Frame

هر Function Call می‌تواند یک **Stack Frame** ایجاد کند.

مثلاً:

```js
function add(a, b) {
    const result = a + b;
    return result;
}

add(10, 20);
```

هنگام اجرای `add`، اطلاعات مرتبط با اجرای آن Function در Stack Frame قرار می‌گیرد.

این اطلاعات می‌تواند شامل مواردی مثل:

- پارامترها
- متغیرهای محلی
- وضعیت اجرای Function
- اطلاعات لازم برای بازگشت از Function

باشد.

---

# بخش بیست‌وششم — Stack مثال ساده

```js
function A() {
    B();
}

function B() {
    C();
}

function C() {
    console.log("Done");
}

A();
```

Stack تقریباً:

```text
┌───────────┐
│ C()       │ ← Top
├───────────┤
│ B()       │
├───────────┤
│ A()       │
├───────────┤
│ Global    │
└───────────┘
```

وقتی `C()` تمام شود:

```text
C removed
```

بعد:

```text
B()
A()
Global
```

---

# بخش بیست‌وهفتم — Heap

**Heap** بخش دیگری از Memory است که Engine برای نگهداری داده‌هایی با طول عمر و اندازه‌ی متغیر از آن استفاده می‌کند.

مثلاً:

```js
const user = {
    name: "Sina",
    age: 23
};
```

می‌توانیم به صورت ساده تصور کنیم:

```text
Stack
  │
  │ user
  ↓
Heap
┌───────────────┐
│ Object        │
│ name: "Sina"  │
│ age: 23       │
└───────────────┘
```

> این یک مدل آموزشی است؛ جزئیات دقیق محل نگهداری همه انواع JavaScript Valueها به Engine و نوع داده و Optimization وابسته است.

---

# بخش بیست‌وهشتم — Stack و Heap چه رابطه‌ای دارند؟

این دو جدا از هم نیستند.

ممکن است یک متغیر یا Reference در Stack باشد ولی به Object موجود در Heap اشاره کند.

مثلاً:

```js
const person = {
    name: "Sina"
};
```

مدل ساده:

```text
CALL STACK

person
  │
  │ reference
  ↓

HEAP

┌────────────────┐
│ Object         │
│ name: "Sina"   │
└────────────────┘
```

پس یک نکته بسیار مهم:

> Reference و Object یکی نیستند.

---

# بخش بیست‌ونهم — Reference چیست؟

فرض کنید:

```js
const user = {
    name: "Sina"
};

const anotherUser = user;
```

حالا:

```text
user ─────────┐
              ↓
           Object
              ↑
anotherUser ──┘
```

هر دو Reference به یک Object اشاره می‌کنند.

اگر بنویسیم:

```js
anotherUser.name = "Ali";
```

آن Object تغییر می‌کند.

پس:

```js
console.log(user.name);
```

خروجی:

```text
Ali
```

است.

---

# بخش سی‌ام — Primitive و Object

در JavaScript با Valueهای Primitive مثل این‌ها مواجهیم:

```js
string
number
bigint
boolean
undefined
symbol
null
```

و Valueهای Object مانند:

```js
object
array
function
```

مدل حافظه‌ی دقیق آن‌ها وابسته به Engine است، بنابراین بهتر است از این جمله مطلق پرهیز کنیم:

> «Primitive همیشه در Stack است و Object همیشه در Heap.»

این یک مدل آموزشی رایج است، اما جزئیات واقعی Engine پیچیده‌تر است.

---

# بخش سی‌ویکم — Garbage Collection

Memory که دیگر استفاده نمی‌شود باید در نهایت آزاد شود.

JavaScript معمولاً Garbage Collection دارد.

یعنی برنامه‌نویس معمولاً لازم نیست دستی بنویسد:

```text
free(object)
```

مثل بعضی زبان‌های مدیریت حافظه دستی.

---

# بخش سی‌ودوم — Garbage Collector

**Garbage Collector (GC)** بررسی می‌کند کدام Objectها هنوز قابل دسترسی هستند.

مدل ساده:

```text
GC Roots
   ↓
Reachable Objects
   ↓
Objects still in use
```

و Objectهایی که دیگر قابل دسترسی نیستند:

```text
Unreachable Objects
        ↓
Garbage
        ↓
Can be collected
```

---

# بخش سی‌وسوم — GC Root

**GC Root** نقطه‌ای است که Garbage Collector از آن برای پیدا کردن Objectهای قابل دسترسی شروع می‌کند.

نمونه‌های مفهومی:

- Global references
- Stack references
- برخی ساختارهای داخلی Runtime
- References مرتبط با Execution Contextها

مثلاً:

```js
const user = {
    name: "Sina"
};
```

اگر `user` از یک Root قابل دسترسی باشد:

```text
GC Root
   ↓
user
   ↓
Object
```

Object قابل دسترسی است.

---

# بخش سی‌وچهارم — Mark and Sweep

یکی از مفاهیم کلاسیک Garbage Collection:

```text
Mark
+
Sweep
```

## Mark

GC از Rootها شروع می‌کند و Objectهای قابل دسترسی را Mark می‌کند.

مثلاً:

```text
Root
 ↓
A
 ↓
B
```

و:

```text
C
```

هیچ Reference قابل دسترسی به آن ندارد.

نتیجه:

```text
A → Marked
B → Marked
C → Unmarked
```

---

# بخش سی‌وپنجم — Sweep

بعد از Mark، GC حافظه‌ای را که متعلق به Objectهای Unreachable است، جمع‌آوری می‌کند.

```text
Marked:
A
B

Unmarked:
C
D

Sweep:
C و D → قابل بازیابی برای Memory
```

مدل کلی:

```text
GC Roots
   ↓
Mark reachable objects
   ↓
Sweep unreachable objects
   ↓
Reclaim memory
```

---

# بخش سی‌وششم — Reference Graph

برای درک GC باید Objectها را به شکل Graph ببینیم.

مثلاً:

```js
const a = {
    child: {
        value: 10
    }
};
```

گراف:

```text
GC Root
   ↓
   a
   ↓
 Object A
   ↓
 Object B
   ↓
 value: 10
```

چون از Root می‌توان به A و سپس B رسید، هر دو Reachable هستند.

---

# بخش سی‌وهفتم — Unreachable Object

فرض کنیم:

```js
let user = {
    name: "Sina"
};

user = null;
```

قبل از انتساب:

```text
Root
 ↓
user
 ↓
Object
```

بعد از:

```js
user = null;
```

اگر هیچ Reference دیگری به Object وجود نداشته باشد:

```text
Root

Object
```

دیگر مسیری از Root به Object وجود ندارد.

بنابراین Object می‌تواند Garbage شود.

---

# بخش سی‌وهشتم — Memory Leak

**Memory Leak** یعنی برنامه به شکلی باعث می‌شود Memory مورد نیاز دیگر آزاد نشود، در حالی که برنامه عملاً دیگر به آن داده نیاز ندارد.

در JavaScript معمولاً مسئله این نیست که GC خراب شده باشد.

اغلب مشکل این است:

> هنوز یک Reference وجود دارد و بنابراین GC تصور می‌کند Object قابل دسترسی و موردنیاز است.

---

# بخش سی‌ونهم — مثال Memory Leak با Array

```js
const data = [];

function addData() {
    data.push(new Array(1000000).fill("data"));
}
```

اگر مرتب:

```js
addData();
```

را اجرا کنیم، Array به رشد خود ادامه می‌دهد.

چون:

```text
Global Root
     ↓
    data
     ↓
Large Objects
```

Objectها هنوز Reachable هستند.

پس GC نمی‌تواند آن‌ها را جمع کند.

---

# بخش چهلم — Memory Leak با Event Listener

مثال مفهومی:

```js
const button = document.querySelector("button");

function handler() {
    console.log("clicked");
}

button.addEventListener("click", handler);
```

اگر Lifecycle مربوط به یک Component یا DOM structure درست مدیریت نشود و Listenerها دائماً اضافه شوند، ممکن است Referenceهای غیرضروری باقی بمانند.

در برخی سناریوها:

```text
Long-lived object
       ↓
Event Listener
       ↓
Other objects
```

می‌تواند باعث زنده ماندن Objectهایی شود که دیگر نباید موردنیاز باشند.

راه‌حل در شرایط مناسب:

```js
button.removeEventListener("click", handler);
```

---

# بخش چهل‌ویکم — Memory Leak با Timer

مثلاً:

```js
const timer = setInterval(() => {
    console.log("running");
}, 1000);
```

اگر Timer دیگر موردنیاز نباشد اما متوقف نشود، Runtime هنوز آن را فعال نگه می‌دارد.

در صورت نیاز:

```js
clearInterval(timer);
```

---

# بخش چهل‌ودوم — Closure و Memory

Closure می‌تواند Referenceهایی را زنده نگه دارد.

مثلاً:

```js
function createCounter() {
    let count = 0;

    return function () {
        count++;
        console.log(count);
    };
}

const counter = createCounter();
```

تابع برگشتی به `count` دسترسی دارد.

بنابراین:

```text
counter
   ↓
Function
   ↓
Closure Environment
   ↓
count
```

تا وقتی `counter` قابل دسترسی باشد، داده موردنیاز Closure نیز می‌تواند زنده بماند.

---

# بخش چهل‌وسوم — آیا Closure باعث Memory Leak است؟

خیر.

Closure خودش Memory Leak نیست.

Closure یک قابلیت عادی و ضروری JavaScript است.

مشکل زمانی رخ می‌دهد که یک Closure ناخواسته Referenceهای سنگین را برای مدت طولانی زنده نگه دارد.

---

# بخش چهل‌وچهارم — Stack Overflow

**Stack Overflow** زمانی رخ می‌دهد که Call Stack بیش از ظرفیت قابل استفاده رشد کند.

مثال معروف:

```js
function recursive() {
    recursive();
}

recursive();
```

چه اتفاقی می‌افتد؟

```text
recursive()
recursive()
recursive()
recursive()
...
```

Stack Frameها دائماً اضافه می‌شوند:

```text
┌─────────────┐
│ recursive() │
├─────────────┤
│ recursive() │
├─────────────┤
│ recursive() │
├─────────────┤
│ recursive() │
├─────────────┤
│ ...         │
└─────────────┘
```

در نهایت ظرفیت Stack تمام می‌شود.

خطایی شبیه:

```text
RangeError: Maximum call stack size exceeded
```

ممکن است دریافت شود.

---

# بخش چهل‌وپنجم — Recursion صحیح

Recursion اگر شرط توقف داشته باشد می‌تواند کاملاً صحیح باشد.

مثلاً:

```js
function countdown(n) {
    if (n <= 0) {
        return;
    }

    console.log(n);
    countdown(n - 1);
}

countdown(5);
```

Call Stack:

```text
countdown(5)
    ↓
countdown(4)
    ↓
countdown(3)
    ↓
countdown(2)
    ↓
countdown(1)
    ↓
countdown(0)
```

بعد از رسیدن به شرط توقف، Functionها یکی‌یکی برمی‌گردند.

---

# بخش چهل‌وششم — تفاوت Stack Overflow و Memory Leak

این دو را با هم اشتباه نکنیم.

## Stack Overflow

مشکل اصلی:

```text
Call Stack بیش از حد رشد کرده
```

مثال:

```js
function loop() {
    loop();
}
```

---

## Memory Leak

مشکل اصلی:

```text
Memory هنوز Reachable است
اما برنامه عملاً دیگر به آن نیاز ندارد
```

مثال:

```js
const cache = [];

setInterval(() => {
    cache.push(new Array(100000));
}, 1000);
```

---

# بخش چهل‌وهفتم — Stack Overflow در برابر Heap Exhaustion

ممکن است Heap هم به سقف مصرف برسد.

مثلاً:

```js
const hugeArray = [];

while (true) {
    hugeArray.push(new Array(1000000));
}
```

در چنین شرایطی مشکل با Stack Overflow فرق دارد.

اینجا Memory Heap در حال پر شدن است.

پس:

```text
Stack Overflow
    ↓
Call Stack problem

Heap exhaustion / Out of Memory
    ↓
Heap / overall memory problem
```

---

# بخش چهل‌وهشتم — یک مثال کامل از مسیر JavaScript

کد:

```js
function greet(name) {
    return "Hello " + name;
}

const message = greet("Sina");

console.log(message);
```

## مرحله 1 — دریافت

```text
Network / Cache / Service Worker
```

فایل JavaScript وارد Browser می‌شود.

## مرحله 2 — Byte Stream

داده به شکل Byte در اختیار Browser قرار می‌گیرد.

## مرحله 3 — Decode

Byteها مطابق Encoding به Source Text تبدیل می‌شوند.

## مرحله 4 — Parse

Parser Syntax را تحلیل می‌کند.

## مرحله 5 — AST

ساختار برنامه به شکل Tree ساخته می‌شود.

مفهوم ساده:

```text
Program
├── FunctionDeclaration
│   ├── greet
│   └── return
│       └── "Hello " + name
│
├── VariableDeclaration
│   └── greet("Sina")
│
└── console.log(message)
```

## مرحله 6 — Bytecode

Engine ساختار قابل اجرای داخلی تولید می‌کند.

## مرحله 7 — Interpreter

Bytecode اجرا می‌شود.

## مرحله 8 — Profiling

Engine رفتار اجرای کد را بررسی می‌کند.

## مرحله 9 — Optimization

اگر بخشی Hot باشد، ممکن است Optimizing Compiler آن را بهینه کند.

## مرحله 10 — Machine Code

برای بخش‌های مناسب Machine Code تولید می‌شود.

## مرحله 11 — CPU

CPU دستورهای Machine Code را اجرا می‌کند.

---

# بخش چهل‌ونهم — مسیر حافظه در همان مثال

کد:

```js
function greet(name) {
    return "Hello " + name;
}

const message = greet("Sina");
```

هنگام اجرای:

```js
greet("Sina");
```

یک Execution Context مربوط به Function ایجاد می‌شود.

به صورت آموزشی:

```text
CALL STACK

greet()
│
├── name → "Sina"
└── local execution data
```

وقتی Function تمام شود، Stack Frame آن حذف می‌شود.

---

# بخش پنجاهم — Heap در همان مثال

اگر Object بسازیم:

```js
const user = {
    name: "Sina"
};
```

مدل مفهومی:

```text
STACK
user
 │
 ↓
HEAP
┌─────────────┐
│ Object      │
│ name: Sina  │
└─────────────┘
```

اگر:

```js
user = null;
```

و Reference دیگری به Object وجود نداشته باشد:

```text
GC Root
  X
  │
Object
```

Object می‌تواند توسط GC جمع‌آوری شود.

---

# بخش پنجاه‌ویکم — یک تصویر ذهنی کامل

تمام مفاهیم را می‌توانیم کنار هم بگذاریم:

```text
                    INTERNET
                       │
                       ↓
              Network / Cache
                       │
                       ↓
                Service Worker
                       │
                       ↓
                  Byte Stream
                       │
                       ↓
              Byte Stream Decoder
                       │
                       ↓
                    Parser
                       │
                       ↓
                     AST
                       │
                       ↓
                   Bytecode
                       │
                       ↓
                 Interpreter
                       │
                       ↓
              Profiler / Feedback
                       │
                       ↓
             Optimizing Compiler
                       │
                       ↓
                Machine Code
                       │
                       ↓
                      CPU


                    MEMORY
                       │
          ┌────────────┴────────────┐
          ↓                         ↓
      Call Stack                  Heap
          │                         │
     Stack Frames             Objects / Data
          │                         │
          └────────────┬────────────┘
                       ↓
                  References
                       ↓
                   GC Roots
                       ↓
                Mark / Sweep
                       ↓
            Unreachable Objects
                       ↓
               Reclaimed Memory
```

---

# بخش پنجاه‌ودوم — چند اشتباه رایج

## اشتباه 1

> JavaScript فقط Interpreter است.

**نادرست یا بیش از حد ساده‌شده است.**

موتورهای مدرن از Interpreter و Compiler و Profiling و Optimization استفاده می‌کنند.

---

## اشتباه 2

> JavaScript فقط Compiler است.

این هم تصویر کاملی نیست.

اجرای JavaScript مدرن معمولاً چند مرحله و چند تکنیک مختلف دارد.

---

## اشتباه 3

> همه Primitiveها در Stack هستند.

این یک ساده‌سازی آموزشی است و نباید به عنوان قانون مطلق Engine در نظر گرفته شود.

---

## اشتباه 4

> همه Objectها در Heap هستند و تمام.

این هم مدل آموزشی است؛ Engine می‌تواند برای Optimization نمایش و محل نگهداری داده‌ها را تغییر دهد.

---

## اشتباه 5

> Garbage Collector هر چیزی را که استفاده نمی‌کنیم فوراً حذف می‌کند.

خیر.

GC معمولاً بر اساس Reachability تصمیم می‌گیرد و زمان دقیق جمع‌آوری را نمی‌توان به شکل ساده «همین الان» فرض کرد.

---

## اشتباه 6

> اگر Object دیگر از نظر منطقی لازم نیست، حتماً Garbage می‌شود.

نه.

اگر هنوز از یک Root قابل دسترسی باشد، از دید GC همچنان Reachable است.

---

# بخش پنجاه‌وسوم — خلاصه واژگان

| اصطلاح | معنی ساده |
|---|---|
| JavaScript Engine | برنامه‌ای که JavaScript را تحلیل و اجرا می‌کند |
| Byte | واحد 8 بیتی داده |
| Byte Stream | جریان Byteها |
| Decoder | تبدیل Byteها به کاراکترها بر اساس Encoding |
| Parser | تحلیل Syntax و Grammar |
| AST | نمایش درختی ساختار کد |
| Bytecode | دستورهای میانی قابل اجرای Engine |
| Interpreter | اجرای Bytecode |
| Profiler | بررسی رفتار اجرای کد |
| Type Feedback | اطلاعاتی درباره Typeهای مشاهده‌شده |
| Optimizing Compiler | Compiler برای بهینه‌سازی کدهای مناسب |
| Machine Code | دستورهای قابل اجرای CPU |
| JIT | Compile کردن در زمان اجرای برنامه |
| Hot Code | کدی که زیاد اجرا می‌شود |
| Deoptimization | خروج از کد بهینه وقتی فرض‌ها دیگر معتبر نیستند |
| Call Stack | ساختار مدیریت Function Callها |
| Stack Frame | اطلاعات مربوط به یک Function Call |
| Heap | ناحیه‌ای برای داده‌های Dynamic و Objectها |
| Reference | اشاره به یک Value/Object |
| Garbage Collector | سیستم مدیریت و جمع‌آوری حافظه |
| GC Root | نقطه شروع پیدا کردن Objectهای Reachable |
| Reachable | قابل دسترسی از Root |
| Unreachable | غیرقابل دسترسی از Root |
| Mark | علامت‌گذاری Objectهای Reachable |
| Sweep | جمع‌آوری Objectهای Unreachable |
| Memory Leak | باقی ماندن ناخواسته حافظه به دلیل Referenceهای زنده |
| Stack Overflow | پر شدن بیش از حد Call Stack |

---

# بخش پنجاه‌وچهارم — مدل نهایی برای به خاطر سپردن

اگر بخواهیم کل درس را در چند خط حفظ کنیم:

```text
1. Browser فایل JavaScript را دریافت می‌کند.
2. Byteها Decode می‌شوند.
3. Parser کد را تحلیل می‌کند.
4. AST ساخته می‌شود.
5. Engine از AST به ساختار اجرایی مانند Bytecode می‌رسد.
6. Interpreter آن را اجرا می‌کند.
7. Profiler و Type Feedback رفتار برنامه را بررسی می‌کنند.
8. کدهای مناسب می‌توانند توسط Optimizing Compiler به Machine Code تبدیل شوند.
9. CPU در نهایت Machine Code را اجرا می‌کند.
10. Call Stack اجرای Functionها را مدیریت می‌کند.
11. Heap داده‌های Dynamic و Objectها را نگهداری می‌کند.
12. Referenceها رابطه بین بخش‌های مختلف Memory را ایجاد می‌کنند.
13. Garbage Collector از GC Rootها شروع می‌کند.
14. Objectهای Reachable زنده می‌مانند.
15. Objectهای Unreachable می‌توانند جمع‌آوری شوند.
16. Memory Leak یعنی داده‌ای که دیگر لازم نیست، به دلیل Referenceهای باقی‌مانده همچنان Reachable مانده باشد.
17. Stack Overflow معمولاً از رشد بیش از حد Call Stack ایجاد می‌شود.
```

---

# بخش پنجاه‌وپنجم — مهم‌ترین رابطه‌ای که باید بفهمی

اگر فقط یک تصویر از این جزوه در ذهنت بماند، این باشد:

```text
             CODE EXECUTION

Source Code
     ↓
   Parser
     ↓
    AST
     ↓
  Bytecode
     ↓
 Interpreter
     ↓
Profiler + Type Feedback
     ↓
Optimizing Compiler
     ↓
Machine Code
     ↓
    CPU


              MEMORY

             Roots
               ↓
          References
               ↓
            Objects
               ↓
        Reachability
               ↓
        Garbage Collector
          ↙         ↘
      Reachable   Unreachable
         ↓             ↓
      Keep          Collect
```

این دو مسیر به هم مرتبط‌اند:

```text
Execution
    ↓
Function Calls
    ↓
Call Stack
    ↓
References
    ↓
Objects in Memory
    ↓
Garbage Collection
```

و به همین دلیل، فهم **Execution Context → Call Stack → Heap → References → GC** برای فهم رفتار واقعی JavaScript بسیار مهم است.
