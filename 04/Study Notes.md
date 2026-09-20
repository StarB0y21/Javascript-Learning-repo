## Created by AI


# جزوه آموزشی: Event Loop، Async/Sync، Queue و Web APIs در JavaScript

> این جزوه ادامه‌ی جزوه‌ی قبلی **JavaScript Engine & Memory Management** است.
>
> هرجا مفهومی از جزوه قبلی دوباره لازم باشد، به بخش مربوطه ارجاع داده شده و از توضیح کامل دوباره‌ی آن خودداری شده است.

---

# بخش اول — پیش‌نیازهای این جزوه

برای فهم Event Loop، چند مفهوم از جزوه قبلی را باید در ذهن داشته باشیم.

### ارجاع به جزوه قبلی

- **JavaScript Engine** → جزوه قبلی، بخش 1
- **Call Stack** → جزوه قبلی، بخش 24 تا 26
- **Stack Frame** → جزوه قبلی، بخش 25
- **Heap** → جزوه قبلی، بخش 27
- **Reference** → جزوه قبلی، بخش 29
- **Garbage Collection** → جزوه قبلی، بخش 31 تا 37
- **Stack Overflow** → جزوه قبلی، بخش 44 تا 45

در این جزوه این مفاهیم را از ابتدا بازنویسی نمی‌کنیم؛ فقط در صورت نیاز از آن‌ها استفاده می‌کنیم.

---

# بخش دوم — JavaScript و Single-Thread

JavaScript در محیط معمولی مرورگر، برای اجرای JavaScript application code یک **Call Stack اصلی** دارد.

به زبان ساده:

```text
JavaScript
    ↓
یک مسیر اصلی اجرای کد
    ↓
Call Stack
```

مثلاً:

```js
console.log("A");
console.log("B");
console.log("C");
```

به ترتیب اجرا می‌شود:

```text
A
B
C
```

اگر یک Function در حال اجرا باشد، Function بعدی نمی‌تواند همان Stack را هم‌زمان اشغال کند.

مثلاً:

```js
function first() {
    second();
}

function second() {
    console.log("Hello");
}

first();
```

Stack به شکل مفهومی:

```text
first()
   ↓
second()
```

جزئیات Call Stack و Stack Frame در **جزوه قبلی، بخش 24 تا 26** آمده است.

> نکته: «JavaScript تک‌ریسمانی است» یک ساده‌سازی مفید برای Main JavaScript Execution Context است. Browser و Node.js خودشان می‌توانند از Threadهای دیگری برای کارهای مختلف استفاده کنند.

---

# بخش سوم — Sync چیست؟

**Synchronous** یعنی عملیات به شکل ترتیبی اجرا شود و اجرای مسیر فعلی برای انجام آن عملیات ادامه پیدا نکند تا آن مرحله تمام شود.

مثلاً:

```js
console.log("A");
console.log("B");
console.log("C");
```

نتیجه:

```text
A
B
C
```

مدل:

```text
Task A
  ↓
Task B
  ↓
Task C
```

---

# بخش چهارم — مثال Synchronous

```js
function first() {
    console.log("First");
}

function second() {
    console.log("Second");
}

first();
second();
```

خروجی:

```text
First
Second
```

چون اجرای `second()` تا زمانی که `first()` تمام نشود شروع نمی‌شود.

---

# بخش پنجم — Async چیست؟

**Asynchronous** یعنی یک عملیات می‌تواند شروع شود و تکمیل آن به زمان دیگری موکول شود، بدون اینکه مسیر اصلی JavaScript مجبور باشد تا پایان آن عملیات در انتظار بماند.

مثلاً:

```js
console.log("A");

setTimeout(() => {
    console.log("B");
}, 1000);

console.log("C");
```

خروجی:

```text
A
C
B
```

برای فهم این مسئله باید Event Loop و Browser APIs را بشناسیم.

---

# بخش ششم — Async به چه معنا نیست؟

این جمله را به خاطر بسپار:

> Async به این معنی نیست که JavaScript روی Call Stack اصلی ناگهان چند قطعه کد JavaScript را هم‌زمان اجرا می‌کند.

بلکه محیط اجرای JavaScript، مانند Browser، امکاناتی خارج از مسیر اجرای مستقیم JavaScript دارد.

مدل ساده:

```text
JavaScript
    ↓
Call Stack

Browser APIs
    ↓
Timer / Network / DOM / ...
```

بعد از آماده شدن نتیجه:

```text
API completion
      ↓
Queue
      ↓
Event Loop
      ↓
Call Stack
```

---

# بخش هفتم — Stack و LIFO

در جزوه قبلی درباره **Call Stack** توضیح داده شد.

اینجا خود مفهوم عمومی **Stack** را بررسی می‌کنیم.

Stack ساختاری است که از قانون:

```text
LIFO
```

استفاده می‌کند.

LIFO یعنی:

```text
Last In
First Out
```

یعنی:

> آخرین چیزی که وارد شده، اولین چیزی است که خارج می‌شود.

---

# بخش هشتم — مثال LIFO

فرض کنیم سه بشقاب روی هم قرار می‌دهیم:

```text
Plate A
Plate B
Plate C
```

اگر ابتدا A، سپس B و سپس C را بگذاریم:

```text
      C ← آخرین ورودی
      B
      A ← اولین ورودی
```

اگر بخواهیم یکی را برداریم، اول C برداشته می‌شود.

پس:

```text
Push A
Push B
Push C

Pop C
Pop B
Pop A
```

---

# بخش نهم — Push و Pop

دو عملیات اصلی Stack:

## Push

قرار دادن یک مقدار روی Stack:

```text
Push(A)
```

## Pop

برداشتن مقدار بالای Stack:

```text
Pop()
```

مثال:

```text
Push A

A

Push B

B ← Top
A

Push C

C ← Top
B
A

Pop

B ← Top
A
```

---

# بخش دهم — ارتباط LIFO با Call Stack

Call Stack جاوااسکریپت نیز از مدل Stack استفاده می‌کند.

مثلاً:

```js
function A() {
    B();
}

function B() {
    C();
}

function C() {
    console.log("Hello");
}

A();
```

مدل:

```text
A()
 ↓
B()
 ↓
C()
```

آخرین Function که وارد Stack شده:

```text
C()
```

اولین Functionی است که تمام می‌شود و از Stack خارج می‌شود.

این همان مفهوم:

```text
LIFO
```

است.

جزئیات Call Stack در **جزوه قبلی، بخش 24 تا 26** آمده است.

---

# بخش یازدهم — Queue و FIFO

**Queue** ساختاری است که معمولاً از قانون:

```text
FIFO
```

استفاده می‌کند.

FIFO یعنی:

```text
First In
First Out
```

یعنی:

> اولین چیزی که وارد Queue شده، اولین چیزی است که خارج می‌شود.

---

# بخش دوازدهم — مثال FIFO

صف نانوایی را تصور کن:

```text
Ali → Reza → Sina → Sara
```

Ali زودتر آمده است.

پس:

```text
Ali
 ↓
Reza
 ↓
Sina
 ↓
Sara
```

Ali اولین نفر خارج می‌شود.

سپس Reza.

سپس Sina.

---

# بخش سیزدهم — Enqueue و Dequeue

در Queue دو مفهوم مهم داریم:

### Enqueue

اضافه کردن به انتهای Queue:

```text
enqueue(A)
enqueue(B)
enqueue(C)
```

نتیجه:

```text
A → B → C
```

### Dequeue

برداشتن از ابتدای Queue:

```text
dequeue()
```

نتیجه:

```text
A
```

و Queue باقی می‌ماند:

```text
B → C
```

---

# بخش چهاردهم — تفاوت Stack و Queue

| ویژگی | Stack | Queue |
|---|---|---|
| قانون | LIFO | FIFO |
| اولین خروج | آخرین ورودی | اولین ورودی |
| عملیات اصلی | Push / Pop | Enqueue / Dequeue |
| مثال | بشقاب‌ها | صف نانوایی |

به صورت تصویری:

```text
STACK

   ↓ Push
 ┌───┐
 │ C │ ← Pop
 ├───┤
 │ B │
 ├───┤
 │ A │
 └───┘


QUEUE

Enqueue → A → B → C → Dequeue
          ↑           ↑
         Front        Back
```

---

# بخش پانزدهم — Web API چیست؟

یکی از مهم‌ترین نکات:

> Web API بخشی از خود زبان JavaScript به معنای ECMAScript نیست؛ Browser به JavaScript مجموعه‌ای از قابلیت‌های محیطی را ارائه می‌دهد.

مثلاً:

```js
setTimeout()
fetch()
document.querySelector()
addEventListener()
localStorage
```

این قابلیت‌ها در محیط Browser در اختیار JavaScript قرار می‌گیرند.

---

# بخش شانزدهم — نمونه Web API

وقتی می‌نویسیم:

```js
setTimeout(() => {
    console.log("Hello");
}, 1000);
```

می‌توانیم به صورت ساده تصور کنیم:

```text
JavaScript
   ↓
setTimeout
   ↓
Browser Timer mechanism
```

JavaScript از Browser درخواست می‌کند:

> این Timer را مدیریت کن.

---

# بخش هفدهم — Web APIهای مهم

در Browser APIهای زیادی وجود دارند.

### Timer APIs

```js
setTimeout()
setInterval()
```

### Network APIs

```js
fetch()
XMLHttpRequest
WebSocket
```

### DOM APIs

```js
document.querySelector()
element.addEventListener()
```

### Storage APIs

```js
localStorage
sessionStorage
```

### APIهای دیگر

```js
Geolocation
Notifications
Web Audio
Canvas
```

همه این APIها دقیقاً یک مدل اجرایی یکسان ندارند، اما همگی نمونه‌ای از قابلیت‌های Host Environment هستند.

---

# بخش هجدهم — setTimeout واقعاً چه می‌کند؟

کد:

```js
setTimeout(() => {
    console.log("Hello");
}, 1000);
```

نباید این‌طور تصور شود:

```text
1000ms صبر کن
↓
Callback را اجرا کن
```

مدل مفهومی بهتر:

```text
setTimeout
    ↓
Timer registration
    ↓
Browser tracks timer
    ↓
Timer becomes eligible
    ↓
Callback enters task scheduling
    ↓
Event Loop
    ↓
Call Stack
    ↓
Callback executes
```

بنابراین:

> `1000ms` معمولاً حداقل تأخیر برای واجد شرایط شدن Callback است، نه تضمین اجرای دقیقاً در میلی‌ثانیه 1000.

---

# بخش نوزدهم — مثال مهم setTimeout

```js
console.log("Start");

setTimeout(() => {
    console.log("Timer");
}, 0);

console.log("End");
```

خروجی:

```text
Start
End
Timer
```

چرا؟

چون:

```js
setTimeout(..., 0)
```

به این معنی نیست:

> Callback همین الان اجرا شود.

Timer باید ثبت شود و Callback وارد چرخه زمان‌بندی شود.

---

# بخش بیستم — Event Loop چیست؟

**Event Loop** سازوکاری در محیط اجرای JavaScript است که به هماهنگ کردن اجرای کارهای آماده از Queueها با Call Stack کمک می‌کند.

مدل ساده:

```text
        ┌───────────────┐
        │   Call Stack  │
        └───────┬───────┘
                ↑
                │
           Event Loop
                ↑
                │
        ┌───────┴───────┐
        │     Queues    │
        └───────────────┘
```

Event Loop به صورت مفهومی بررسی می‌کند که چه زمانی کار بعدی می‌تواند وارد چرخه اجرای JavaScript شود.

---

# بخش بیست‌ویکم — Event Loop در یک جمله

اگر بخواهیم خیلی ساده بگوییم:

> Event Loop هماهنگ می‌کند که چه زمانی کارهای آماده‌شده از Queueهای مربوط به Event Loop بتوانند روی Call Stack اجرا شوند.

---

# بخش بیست‌ودوم — مثال کامل Event Loop

کد:

```js
console.log("1");

setTimeout(() => {
    console.log("2");
}, 0);

console.log("3");
```

ابتدا:

```text
Call Stack
console.log("1")
```

خروجی:

```text
1
```

سپس Timer ثبت می‌شود.

بعد:

```text
Call Stack
console.log("3")
```

خروجی:

```text
3
```

بعد از آماده شدن Timer:

```text
Task Queue
   ↓
Event Loop
   ↓
Call Stack
   ↓
console.log("2")
```

خروجی نهایی:

```text
1
3
2
```

---

# بخش بیست‌وسوم — Callback چیست؟

**Callback** تابعی است که به کد دیگری داده می‌شود تا در زمان مناسب بعداً اجرا شود.

مثلاً:

```js
function greet() {
    console.log("Hello");
}

setTimeout(greet, 1000);
```

اینجا:

```text
greet
```

یک Callback است.

یعنی:

> این Function را بعداً، وقتی شرایط اجرای آن فراهم شد، اجرا کن.

---

# بخش بیست‌وچهارم — Callback Queue

اصطلاح **Callback Queue** یک اصطلاح آموزشی رایج برای توضیح صفی است که Callbackهای آماده‌ی اجرای JavaScript در آن منتظر می‌مانند.

مدل ساده:

```text
Timer / Event
      ↓
Callback
      ↓
Callback Queue
      ↓
Event Loop
      ↓
Call Stack
```

اما یک نکته مهم وجود دارد:

> در Browser مدرن بهتر است تصور نکنیم فقط یک Queue وجود دارد.

حداقل دو مفهوم مهم برای این مبحث:

```text
Task Queue
Microtask Queue
```

را باید بشناسیم.

---

# بخش بیست‌وپنجم — Task

**Task** یک واحد کاری است که در چرخه Event Loop اجرا می‌شود.

نمونه‌هایی از کارهایی که می‌توانند به عنوان Task وارد چرخه شوند:

- Timer callbacks
- برخی DOM events
- برخی عملیات محیط Browser

مثلاً:

```js
setTimeout(() => {
    console.log("Timer");
}, 0);
```

Callback مربوط به Timer در مدل آموزشی ما یک Task محسوب می‌شود.

---

# بخش بیست‌وششم — Microtask Queue

این مفهوم در فهرست اولیه تو نبود، اما برای فهم `Promise` و `async/await` **ضروری است**.

Microtaskها Queue جداگانه‌ای دارند.

مثلاً:

```js
Promise.resolve().then(() => {
    console.log("Promise");
});
```

تابع داخل `.then()` یک Microtask است که پس از fulfilled شدن Promise در Microtask Queue قرار می‌گیرد.

---

# بخش بیست‌وهفتم — Task Queue در برابر Microtask Queue

مثال:

```js
console.log("A");

setTimeout(() => {
    console.log("B");
}, 0);

Promise.resolve().then(() => {
    console.log("C");
});

console.log("D");
```

خروجی:

```text
A
D
C
B
```

ابتدا کد synchronous اجرا می‌شود:

```text
A
D
```

سپس Microtask:

```text
C
```

و سپس Task مربوط به Timer:

```text
B
```

مدل ساده:

```text
Synchronous Code
       ↓
Microtasks
       ↓
Tasks
```

---

# بخش بیست‌وهشتم — چرا Microtask مهم است؟

فرض کنیم:

```js
Promise.resolve().then(() => {
    console.log("Microtask");
});

setTimeout(() => {
    console.log("Task");
}, 0);
```

خروجی معمول:

```text
Microtask
Task
```

زیرا Microtaskها در نقطه مناسب از چرخه Event Loop قبل از رفتن به Task بعدی تخلیه می‌شوند.

---

# بخش بیست‌ونهم — Promise

Promise یک Object است که نتیجه آینده‌ی یک عملیات asynchronous را نمایندگی می‌کند.

سه حالت اصلی:

```text
pending
fulfilled
rejected
```

مثلاً:

```js
const promise = fetch("/users");
```

Promise در ابتدا می‌تواند:

```text
pending
```

باشد.

بعد:

```text
fulfilled
```

یا:

```text
rejected
```

شود.

---

# بخش سی‌ام — then

مثلاً:

```js
fetch("/users")
    .then(response => {
        console.log(response);
    });
```

تابع داخل:

```js
.then(...)
```

زمانی که Promise fulfilled شود، به عنوان یک Promise reaction در مسیر Microtask اجرا می‌شود.

---

# بخش سی‌ویکم — async

Keyword:

```js
async
```

برای تعریف یک Async Function استفاده می‌شود.

مثلاً:

```js
async function getUser() {
    return "Sina";
}
```

از دید caller، نتیجه یک Async Function یک Promise است.

مثلاً:

```js
const result = getUser();

console.log(result);
```

`result` یک Promise خواهد بود، نه مستقیماً String.

---

# بخش سی‌ودوم — await

`await` اجازه می‌دهد داخل یک `async function` منتظر نتیجه‌ی Promise بمانیم، بدون اینکه کل مسیر اجرای JavaScript را برای آن مدت قفل کنیم.

مثلاً:

```js
async function getData() {
    const response = await fetch("/users");

    console.log(response);
}
```

وقتی اجرای Function به:

```js
await fetch("/users");
```

می‌رسد، ادامه Function وابسته به Promise می‌شود.

این به معنی:

> Call Stack را برای چند ثانیه قفل کن.

نیست.

---

# بخش سی‌وسوم — مدل ذهنی await

فرض کنیم:

```js
async function test() {
    console.log("A");

    await Promise.resolve();

    console.log("B");
}

test();

console.log("C");
```

خروجی:

```text
A
C
B
```

چرا؟

ابتدا:

```text
test()
 ↓
console.log("A")
```

پس:

```text
A
```

سپس ادامه Function بعد از `await` برای اجرای بعدی برنامه‌ریزی می‌شود.

کد synchronous بیرونی ادامه پیدا می‌کند:

```js
console.log("C");
```

پس:

```text
C
```

بعد continuation مربوط به `await` به صورت Microtask اجرا می‌شود:

```text
B
```

---

# بخش سی‌وچهارم — await قفل کردن Thread نیست

این نکته بسیار مهم است.

کد:

```js
const response = await fetch("/users");
```

به این معنی نیست:

```text
JavaScript
   ↓
STOP
   ↓
10 seconds
   ↓
Continue
```

بلکه مدل مفهومی:

```text
async function
      ↓
await Promise
      ↓
Continue later
      ↓
Other work can run
      ↓
Promise settles
      ↓
Continuation becomes a Microtask
      ↓
Event Loop
      ↓
Call Stack
      ↓
Function continues
```

---

# بخش سی‌وپنجم — مثال async/await کامل

```js
async function loadUser() {
    console.log("Start");

    const response = await fetch("/user");

    console.log("User loaded");
}

console.log("Before");

loadUser();

console.log("After");
```

ابتدا:

```text
Before
```

سپس:

```text
Start
```

بعد `fetch` عملیات asynchronous را شروع می‌کند و Function در `await` ادامه‌ی خود را به آینده موکول می‌کند.

اما Call Stack می‌تواند کارهای دیگر را انجام دهد.

بنابراین:

```text
After
```

بعد از تکمیل Promise، ادامه Function اجرا می‌شود:

```text
User loaded
```

---

# بخش سی‌وششم — Heap و Event Loop

در جزوه قبلی:

- Heap → بخش 27
- Reference → بخش 29
- Garbage Collection → بخش 31 تا 37

توضیح داده شد.

اینجا نکته مهم این است که Async Code می‌تواند Objectهایی ایجاد کند که تا زمان تکمیل عملیات asynchronous مورد نیاز باشند.

مثلاً:

```js
async function process() {
    const data = {
        name: "Sina"
    };

    await fetch("/data");

    console.log(data.name);
}
```

برای ادامه‌ی Function، Runtime باید وضعیت و داده‌های لازم را تا زمانی که مورد نیازند حفظ کند.

---

# بخش سی‌وهفتم — Event Loop و Heap یکی نیستند

این مفاهیم را جدا نگه دار:

```text
Call Stack
Heap
Web APIs
Queues
Event Loop
```

هرکدام نقش متفاوتی دارند.

### Call Stack

مدیریت اجرای Functionها.

### Heap

مدیریت Memory برای داده‌های Dynamic و Objectها.

### Web APIs

قابلیت‌های Host Browser.

### Queue

نگهداری کارهای آماده یا قابل اجرای بعدی.

### Event Loop

هماهنگی چرخه اجرای JavaScript با Queueها و Host.

---

# بخش سی‌وهشتم — Trace چیست؟

واژه‌ی **Trace** در این مبحث یعنی دنبال کردن مرحله‌به‌مرحله‌ی اجرای برنامه.

مثلاً:

```js
console.log("A");

setTimeout(() => {
    console.log("B");
}, 0);

console.log("C");
```

می‌توانیم Trace بسازیم.

### مرحله 1

```text
Call Stack:
console.log("A")
```

خروجی:

```text
A
```

### مرحله 2

```text
setTimeout(...)
```

Timer در Browser ثبت می‌شود.

### مرحله 3

```text
Call Stack:
console.log("C")
```

خروجی:

```text
C
```

### مرحله 4

Timer آماده می‌شود.

Callback وارد مسیر Task scheduling می‌شود.

### مرحله 5

Event Loop بررسی می‌کند که Call Stack اجازه‌ی اجرای Task را دارد.

### مرحله 6

Callback وارد Call Stack می‌شود:

```text
console.log("B")
```

خروجی:

```text
B
```

نتیجه:

```text
A
C
B
```

---

# بخش سی‌ونهم — Trace با Stack

یک روش بسیار خوب برای یادگیری Event Loop این است که در هر لحظه وضعیت Stack و Queue را بنویسیم.

کد:

```js
console.log("A");

setTimeout(() => {
    console.log("B");
}, 0);

console.log("C");
```

### لحظه اول

```text
STACK
┌───────────────────┐
│ console.log("A")  │
└───────────────────┘

QUEUE
Empty
```

خروجی:

```text
A
```

---

### لحظه دوم

Timer ثبت می‌شود:

```text
STACK
┌───────────────────┐
│ global execution  │
└───────────────────┘

Browser Timer
┌───────────────────┐
│ setTimeout        │
└───────────────────┘
```

---

### لحظه سوم

```text
STACK
┌───────────────────┐
│ console.log("C")  │
└───────────────────┘
```

خروجی:

```text
C
```

---

### لحظه چهارم

Timer آماده شده است:

```text
Task Queue

┌─────────────────────┐
│ callback B          │
└─────────────────────┘
```

---

### لحظه پنجم

Event Loop اجازه می‌دهد Callback وارد Stack شود:

```text
STACK

┌─────────────────────┐
│ callback B          │
└─────────────────────┘
```

خروجی:

```text
B
```

---

# بخش چهلم — Trace با Promise و Timer

کد:

```js
console.log("A");

setTimeout(() => {
    console.log("B");
}, 0);

Promise.resolve().then(() => {
    console.log("C");
});

console.log("D");
```

### Trace

ابتدا:

```text
A
```

سپس Timer ثبت می‌شود.

Promise reaction نیز برای Microtask scheduling آماده می‌شود.

بعد:

```text
D
```

وقتی synchronous execution تمام شد:

```text
Microtask Queue
┌──────────────┐
│ console.log C│
└──────────────┘

Task Queue
┌──────────────┐
│ console.log B│
└──────────────┘
```

Microtask ابتدا اجرا می‌شود:

```text
C
```

سپس Task:

```text
B
```

نتیجه:

```text
A
D
C
B
```

---

# بخش چهل‌ویکم — Event Loop و Rendering

در Browser فقط JavaScript وجود ندارد.

Browser باید کارهایی مثل:

- Layout
- Paint
- Rendering
- Input handling

را نیز مدیریت کند.

بنابراین Event Loop بخشی از یک سیستم بزرگ‌تر در Browser است.

در نتیجه نمودار آموزشی:

```text
Call Stack → Web API → Callback Queue → Event Loop
```

برای شروع مفید است، اما تصویر کامل معماری Browser نیست.

---

# بخش چهل‌ودوم — آیا Web API خودش Queue است؟

خیر.

این‌ها را جدا نگه دار:

```text
Web API
```

یعنی قابلیت Browser.

و:

```text
Queue
```

یعنی ساختاری برای نگهداری کارهایی که در چرخه اجرا منتظرند.

مثلاً:

```text
setTimeout
   ↓
Browser Timer
   ↓
callback becomes eligible
   ↓
Task scheduling
   ↓
Event Loop
   ↓
Call Stack
```

---

# بخش چهل‌وسوم — آیا Event Loop کد JavaScript را اجرا می‌کند؟

بهتر است بگوییم:

> Event Loop خودش محل اجرای JavaScript Function نیست.

کار اصلی آن هماهنگ کردن چرخه‌ی اجرای کارهای قابل اجراست.

اجرای واقعی JavaScript Function توسط JavaScript Engine و روی Call Stack انجام می‌شود.

مدل:

```text
Event Loop
    ↓
coordinates
    ↓
Call Stack
    ↓
JavaScript Engine executes
```

---

# بخش چهل‌وچهارم — چرا JavaScript می‌تواند Responsive بماند؟

فرض کنیم:

```js
fetch("/large-data");
```

اگر JavaScript مجبور بود خودش تا پایان Network Request روی Call Stack بماند:

```text
Call Stack
    ↓
WAIT
    ↓
10 seconds
    ↓
Continue
```

صفحه می‌توانست برای مدت طولانی پاسخ‌گو نباشد.

اما در مدل asynchronous:

```text
JavaScript
    ↓
Start Network operation
    ↓
Browser handles I/O
    ↓
JavaScript continues
```

وقتی نتیجه آماده شد:

```text
Network completion
      ↓
Queue
      ↓
Event Loop
      ↓
Call Stack
```

---

# بخش چهل‌وپنجم — Blocking

**Blocking** یعنی کاری مانع ادامه اجرای کارهای دیگر روی همان مسیر اجرای اصلی شود.

مثلاً:

```js
function heavyTask() {
    for (let i = 0; i < 10_000_000_000; i++) {
        // heavy work
    }
}

heavyTask();

console.log("Done");
```

تا وقتی `heavyTask()` روی Call Stack مشغول است، اجرای:

```js
console.log("Done");
```

نمی‌تواند انجام شود.

---

# بخش چهل‌وششم — چرا Async همیشه به معنی Non-Blocking نیست؟

این یک اشتباه رایج است.

فقط اضافه کردن:

```js
async
```

یک عملیات CPU-intensive را جادویی از Call Stack خارج نمی‌کند.

مثلاً:

```js
async function test() {
    for (let i = 0; i < 10_000_000_000; i++) {
        // heavy CPU work
    }
}
```

این Loop همچنان می‌تواند Main Thread را Block کند.

`async` بیشتر برای مدل Promise-based asynchronous programming است.

---

# بخش چهل‌وهفتم — Workerها

اگر CPU work واقعاً سنگین باشد، Browser می‌تواند از Web Worker استفاده کند.

مدل:

```text
Main Thread
    │
    ├── JavaScript
    ├── Event Loop
    └── UI
          │
          │ Message
          ↓
     Web Worker
          │
          └── Heavy computation
```

این موضوع با Event Loop اصلی مرتبط است اما مبحث مستقلی محسوب می‌شود.

---

# بخش چهل‌وهشتم — Stack Overflow و Event Loop

در جزوه قبلی، **Stack Overflow** در بخش 44 و 45 توضیح داده شد.

Event Loop نمی‌تواند جلوی Recursion بی‌نهایت را بگیرد.

مثلاً:

```js
function loop() {
    loop();
}

loop();
```

Call Stack دائماً رشد می‌کند:

```text
loop()
loop()
loop()
loop()
...
```

در نهایت:

```text
Maximum call stack size exceeded
```

---

# بخش چهل‌ونهم — یک مثال ترکیبی کامل

کد:

```js
console.log("1");

setTimeout(() => {
    console.log("2");
}, 0);

Promise.resolve().then(() => {
    console.log("3");
});

async function test() {
    console.log("4");

    await Promise.resolve();

    console.log("5");
}

test();

console.log("6");
```

خروجی:

```text
1
4
6
3
5
2
```

بیایید Trace کنیم.

### مرحله 1

```js
console.log("1");
```

خروجی:

```text
1
```

### مرحله 2

```js
setTimeout(...)
```

Timer ثبت می‌شود.

### مرحله 3

```js
Promise.resolve().then(...)
```

Promise reaction برای Microtask scheduling آماده می‌شود.

### مرحله 4

```js
test();
```

Function اجرا می‌شود:

```js
console.log("4");
```

پس:

```text
4
```

### مرحله 5

اجرای Function به:

```js
await Promise.resolve();
```

می‌رسد.

ادامه:

```js
console.log("5");
```

برای اجرای بعدی در قالب Microtask ادامه پیدا می‌کند.

### مرحله 6

کد synchronous بیرونی ادامه پیدا می‌کند:

```js
console.log("6");
```

پس:

```text
1
4
6
```

### مرحله 7

Microtask مربوط به `.then()` اجرا می‌شود:

```text
3
```

### مرحله 8

Microtask مربوط به continuation `await` اجرا می‌شود:

```text
5
```

### مرحله 9

Task مربوط به Timer اجرا می‌شود:

```text
2
```

نتیجه:

```text
1
4
6
3
5
2
```

---

# بخش پنجاهم — تصویر ذهنی نهایی

```text
                       JAVASCRIPT
                            │
                            ↓
                       Call Stack
                            │
              ┌─────────────┴─────────────┐
              │                           │
              ↓                           ↓
          Synchronous                 Async APIs
              │                    Browser / Host
              │                           │
              │                  ┌────────┴────────┐
              │                  ↓                 ↓
              │               Timer             Network
              │                  │                 │
              │                  └────────┬────────┘
              │                           ↓
              │                      Task Queue
              │
              │                      Microtask Queue
              │                              ↑
              │                         Promise / await
              │
              └──────────────┬───────────────┘
                             ↓
                         Event Loop
                             ↓
                         Call Stack
                             ↓
                         Execution
```

---

# بخش پنجاه‌ویکم — مدل بسیار خلاصه Event Loop

برای حفظ کردن:

```text
1. JavaScript Code
2. Call Stack
3. Async operation
4. Browser / Host API
5. Callback becomes ready
6. Queue
7. Event Loop
8. Call Stack
9. Callback executes
```

و برای Promise:

```text
JavaScript
    ↓
Promise
    ↓
Microtask Queue
    ↓
Event Loop checkpoint
    ↓
Call Stack
```

---

# بخش پنجاه‌ودوم — نکات بسیار مهم

## 1. `setTimeout(fn, 0)` یعنی فوراً اجرا نمی‌شود

بلکه:

```text
minimum delay / eligibility
+
queue scheduling
+
waiting for the right execution point
```

---

## 2. `async` به معنی Thread جدید نیست

این:

```js
async function test() {}
```

به خودی خود Thread جدید ایجاد نمی‌کند.

---

## 3. `await` به معنی قفل شدن کل برنامه نیست

`await` اجازه می‌دهد ادامه Async Function بعداً ادامه پیدا کند.

---

## 4. Web API با JavaScript Engine یکی نیست

```text
JavaScript Engine
```

مسئول اجرای JavaScript است.

```text
Browser APIs
```

قابلیت‌های Host را فراهم می‌کنند.

---

## 5. Callback Queue تنها Queue موجود نیست

برای فهم مدرن Browser باید حداقل این دو مفهوم را بشناسی:

```text
Task Queue
Microtask Queue
```

---

## 6. Event Loop خودش Call Stack نیست

Event Loop وظیفه اجرای مستقیم Function را ندارد.

---

## 7. Heap و Call Stack را با Queue قاطی نکن

```text
Call Stack → execution state

Heap → dynamic memory / objects

Queue → pending work

Event Loop → coordination
```

---

# بخش پنجاه‌وسوم — واژه‌نامه

| اصطلاح | معنی |
|---|---|
| Synchronous | اجرای ترتیبی |
| Asynchronous | تکمیل عملیات در زمان بعدی بدون نگه داشتن مسیر اصلی در انتظار |
| Stack | ساختار LIFO |
| LIFO | Last In, First Out |
| Queue | ساختار FIFO |
| FIFO | First In, First Out |
| Push | اضافه کردن به Stack |
| Pop | برداشتن از Stack |
| Enqueue | اضافه کردن به Queue |
| Dequeue | برداشتن از Queue |
| Call Stack | ساختار مدیریت Function Callها |
| Callback | تابعی که برای اجرای بعدی تحویل داده می‌شود |
| Callback Queue | اصطلاح آموزشی برای صف Callbackهای آماده |
| Task | واحد کاری در چرخه Event Loop |
| Task Queue | صف Taskها |
| Microtask | واحد کاری با اولویت اجرای متفاوت، مانند Promise reactions |
| Microtask Queue | صف Microtaskها |
| Web API | قابلیت‌های Browser/Host در اختیار JavaScript |
| Event Loop | سازوکار هماهنگ‌کننده چرخه اجرای Eventها و Queueها |
| Promise | نماینده نتیجه آینده یک عملیات asynchronous |
| async | تعریف Async Function |
| await | انتظار منطقی برای Promise در Async Function |
| Blocking | جلوگیری از ادامه اجرای کارهای دیگر روی همان مسیر |
| Trace | دنبال کردن مرحله‌به‌مرحله اجرای برنامه |
| Timer | سازوکار زمان‌بندی مانند setTimeout |
| Main Thread | مسیر اصلی اجرای JS/UI در Browser |
| Web Worker | محیط جداگانه برای اجرای JavaScript در Worker |

---

# بخش پنجاه‌وچهارم — جمع‌بندی نهایی

اگر بخواهیم کل این جزوه را در یک سناریو خلاصه کنیم:

کد:

```js
console.log("Start");

setTimeout(() => {
    console.log("Timer");
}, 0);

Promise.resolve().then(() => {
    console.log("Promise");
});

console.log("End");
```

مسیر مفهومی:

```text
             JavaScript Code
                    │
                    ↓
               Call Stack
                    │
       ┌────────────┴────────────┐
       │                         │
       ↓                         ↓
  Synchronous                 Async
       │                         │
 Start / End              Timer / Promise
                                 │
                    ┌────────────┴────────────┐
                    ↓                         ↓
               Task Queue              Microtask Queue
                    │                         │
                    └────────────┬────────────┘
                                 ↓
                            Event Loop
                                 ↓
                            Call Stack
                                 ↓
                              Execute
```

خروجی:

```text
Start
End
Promise
Timer
```

پس رابطه اصلی را این‌گونه به خاطر بسپار:

```text
CALL STACK
    ↓
executes JavaScript

WEB APIs
    ↓
handle host capabilities

QUEUES
    ↓
hold work waiting for execution

EVENT LOOP
    ↓
coordinates when queued work can run

CALL STACK
    ↓
executes the callback
```

و مهم‌تر از همه:

```text
Stack  = LIFO
Queue  = FIFO
Call Stack = execution
Heap = memory
Web API = browser capabilities
Microtask Queue = Promise/await-related continuation
Task Queue = tasks such as timer callbacks
Event Loop = coordination
```

این مدل ذهنی پایه‌ی فهم بسیاری از رفتارهای عجیب JavaScript است؛ مخصوصاً اینکه چرا خروجی برنامه همیشه مطابق ترتیب ظاهری خطوط کد نیست.
