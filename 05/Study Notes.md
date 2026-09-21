## Created by AI


# جزوه آموزشی JavaScript — Variables، `var`، `let`، `const` و Naming Rules

> این جزوه ادامه‌ی جزوه‌های قبلی JavaScript است و مفاهیم Variable، Declaration، Assignment، Reassignment، Restriction و Naming Rules را با مثال بررسی می‌کند.

---

## 1. Variable چیست؟

**Variable** نامی است که برای دسترسی به یک مقدار در برنامه استفاده می‌کنیم.

```js
let age = 23;
```

اینجا:

```text
age → نام Variable
23  → مقدار
```

مثال:

```js
let username = "Sina";

console.log(username);
```

خروجی:

```text
Sina
```

---

## 2. Declare Variable — تعریف/اعلام Variable

**Declaration** یعنی به JavaScript اعلام کنیم که یک Variable Binding با نام مشخص وجود دارد.

```js
let age;
```

اینجا `age` Declare شده، اما هنوز مقدار اولیه‌ای به آن نداده‌ایم.

```js
console.log(age);
```

خروجی:

```text
undefined
```

### مثال دیگر

```js
let username;
let score;
let isLoggedIn;
```

هر سه Variable Declare شده‌اند.

---

## 3. Initialization — مقداردهی اولیه

**Initialization** یعنی اولین مقدار را هنگام Declaration به Variable بدهیم.

```js
let age = 23;
```

در این خط:

```text
Declaration → age
Initialization → 23
```

مقایسه:

```js
let age;       // Declaration
age = 23;      // Assignment
```

در برابر:

```js
let age = 23;  // Declaration + Initialization
```

---

## 4. Assign to Variable — Assignment

**Assignment** یعنی یک مقدار را به Variable نسبت بدهیم.

```js
let age;

age = 23;
```

علامت:

```js
=
```

در اینجا **Assignment Operator** است.

مدل ذهنی:

```text
age
 ↓
23
```

مثال:

```js
let username;

username = "Sina";

console.log(username);
```

خروجی:

```text
Sina
```

---

## 5. Reassign to Variable — Reassignment

**Reassignment** یعنی مقدار Variable موجود را با مقدار دیگری جایگزین کنیم.

```js
let age = 23;

age = 24;
```

ابتدا:

```text
age → 23
```

بعد:

```text
age → 24
```

خط دوم Reassignment است.

### تفاوت

```js
let age;    // Declaration

age = 23;   // Assignment

age = 24;   // Reassignment
```

---

## 6. Redeclaration چیست؟

**Redeclaration** یعنی دوباره همان Variable را در Scope مشابه Declare کنیم.

مثلاً:

```js
let age = 23;
let age = 24;
```

خطا می‌دهد.

اما:

```js
let age = 23;
age = 24;
```

مجاز است.

پس:

```text
Redeclaration ≠ Reassignment
```

---

# 7. سه Keyword اصلی

JavaScript سه Keyword اصلی برای تعریف Variable دارد:

```js
var
let
const
```

مثال:

```js
var oldName = "Sina";

let score = 10;

const birthYear = 1380;
```

رفتار این سه متفاوت است.

---

# 8. `let`

`let` برای Variableای مناسب است که ممکن است بعداً مقدارش تغییر کند.

```js
let score = 10;

score = 20;

console.log(score);
```

خروجی:

```text
20
```

پس:

```text
let → Reassignment مجاز است
```

### مثال کاربردی

```js
let lives = 3;

lives = 2;
lives = 1;
lives = 0;
```

چون مقدار `lives` در طول برنامه تغییر می‌کند، `let` مناسب است.

---

# 9. `const`

`const` برای Bindingای است که بعد از Initialization نمی‌توان آن را Reassign کرد.

```js
const birthYear = 1380;
```

این کد خطا می‌دهد:

```js
birthYear = 1381;
```

چون:

```text
const → Reassignment ممنوع
```

---

## 10. `const` باید مقدار اولیه داشته باشد

این کد معتبر نیست:

```js
const age;
```

اما این معتبر است:

```js
const age = 23;
```

پس:

```text
let
→ می‌تواند بدون مقدار اولیه Declare شود

const
→ باید هنگام Declaration مقدار اولیه داشته باشد
```

---

# 11. آیا `const` یعنی Object غیرقابل تغییر است؟

خیر.

این کد:

```js
const user = {
    name: "Sina"
};
```

اجازه این کار را نمی‌دهد:

```js
user = {
    name: "Ali"
};
```

اما این مجاز است:

```js
user.name = "Ali";
```

چون `const` مانع Reassignment خود Binding می‌شود، نه لزوماً تغییر محتوای Object.

مدل ساده:

```text
user
 ↓
Object
 ↓
name: "Sina"
```

بعد:

```text
user
 ↓
همان Object
 ↓
name: "Ali"
```

این موضوع با **Heap و Reference** مرتبط است؛ برای توضیح کامل به بخش Heap و Reference در جزوه قبلی مراجعه کن.

---

# 12. مثال `const` با Array

```js
const numbers = [1, 2, 3];

numbers.push(4);

console.log(numbers);
```

خروجی:

```text
[1, 2, 3, 4]
```

اما:

```js
numbers = [10, 20];
```

خطا است.

بنابراین:

```text
const Array → خود Binding قابل Reassign نیست
             اما Array mutable ممکن است تغییر کند
```

---

# 13. `var`

`var` روش قدیمی‌تر تعریف Variable در JavaScript است.

```js
var age = 23;

age = 24;
```

Reassignment مجاز است.

اما تفاوت مهم آن با `let` و `const` مربوط به Scope است.

---

# 14. Scope چیست؟

**Scope** محدوده‌ای است که در آن یک Identifier قابل دسترسی است.

مثلاً:

```js
{
    let age = 23;

    console.log(age);
}
```

داخل Block می‌توان `age` را استفاده کرد.

اما:

```js
{
    let age = 23;
}

console.log(age);
```

خطا می‌دهد.

---

# 15. Block چیست؟

Block معمولاً با `{}` مشخص می‌شود.

مثلاً:

```js
{
    let name = "Sina";
}
```

همچنین:

```js
if (true) {
    let score = 10;
}
```

و:

```js
for (let i = 0; i < 3; i++) {
    console.log(i);
}
```

هر قسمت `{ ... }` یک Block ایجاد می‌کند.

---

# 16. Block Scope

`let` و `const` **Block Scoped** هستند.

مثال:

```js
{
    let age = 23;
    const name = "Sina";

    console.log(age);
    console.log(name);
}
```

اما بیرون:

```js
{
    let age = 23;
}

console.log(age);
```

`age` قابل دسترسی نیست و `ReferenceError` دریافت می‌شود.

---

# 17. Function Scope

`var` **Function Scoped** است.

مثال:

```js
function test() {
    var age = 23;

    console.log(age);
}

test();
```

اما:

```js
function test() {
    var age = 23;
}

console.log(age);
```

خطا می‌دهد.

یعنی `var` به Function محدود است.

---

# 18. تفاوت `var` با Block Scope

```js
{
    var a = 10;
    let b = 20;
    const c = 30;
}

console.log(a);
```

`a` قابل دسترسی است.

اما:

```js
console.log(b);
console.log(c);
```

خارج از Block قابل دسترسی نیستند.

پس:

```text
var   → Function Scope
let   → Block Scope
const → Block Scope
```

---

# 19. مثال مهم Scope

```js
function test() {
    if (true) {
        var a = 10;
        let b = 20;
        const c = 30;
    }

    console.log(a); // 10
    console.log(b); // ReferenceError
    console.log(c); // ReferenceError
}
```

دلیل:

```text
a → با var تعریف شده → Function Scoped
b → با let تعریف شده → Block Scoped
c → با const تعریف شده → Block Scoped
```

---

# 20. Restriction — محدودیت‌ها

**Restriction** یعنی محدودیتی که یک Keyword یا ساختار روی Variable اعمال می‌کند.

مقایسه:

| ویژگی | `var` | `let` | `const` |
|---|---|---|---|
| Reassign | بله | بله | خیر |
| Initialization اجباری | خیر | خیر | بله |
| Block Scoped | خیر | بله | بله |
| Function Scoped | بله | خیر | خیر |
| Redeclare در همان Scope | بله | خیر | خیر |

---

# 21. Redeclaration با `var`

این کد مجاز است:

```js
var name = "Sina";

var name = "Ali";

console.log(name);
```

خروجی:

```text
Ali
```

این رفتار یکی از ویژگی‌های قدیمی `var` است.

---

# 22. Redeclaration با `let`

این کد خطا می‌دهد:

```js
let name = "Sina";

let name = "Ali";
```

اما:

```js
let name = "Sina";

name = "Ali";
```

مجاز است.

---

# 23. Redeclaration با `const`

این نیز خطا می‌دهد:

```js
const name = "Sina";

const name = "Ali";
```

و این هم خطاست:

```js
const name = "Sina";

name = "Ali";
```

اولی مشکل **Redeclaration** دارد و دومی مشکل **Reassignment**.

---

# 24. Temporal Dead Zone — TDZ

`let` و `const` یک رفتار مهم دیگر دارند: **Temporal Dead Zone**.

مثلاً:

```js
console.log(age);

let age = 23;
```

خطا:

```text
ReferenceError
```

به فاصله‌ای که Variable از ابتدای Scope تا رسیدن اجرای برنامه به Declaration در وضعیت قابل‌استفاده‌نبودن قرار دارد، TDZ گفته می‌شود.

---

# 25. TDZ با مثال

این کد خطاست:

```js
{
    console.log(name);

    let name = "Sina";
}
```

اما این درست است:

```js
{
    let name = "Sina";

    console.log(name);
}
```

---

# 26. `var` و Hoisting

رفتار `var` متفاوت است.

```js
console.log(age);

var age = 23;
```

خروجی:

```text
undefined
```

به شکل ساده می‌توان رفتار را چنین تصور کرد:

```js
var age;

console.log(age);

age = 23;
```

این یک مدل آموزشی از **Hoisting** است.

---

# 27. `let` و Hoisting

`let` و `const` نیز در فرآیند ایجاد Environment قبل از اجرای خط Declaration ثبت می‌شوند، اما قبل از رسیدن اجرا به Declaration در TDZ قرار دارند.

برای استفاده عملی این مدل را حفظ کن:

```text
var
→ دسترسی پیش از Declaration در چنین سناریویی
→ undefined

let / const
→ TDZ
→ ReferenceError
```

---

# 28. Naming Rules — قوانین نام‌گذاری

نام Variable باید قوانین Identifierهای JavaScript را رعایت کند.

می‌توانیم از حروف، اعداد، `_` و `$` استفاده کنیم، با محدودیت‌های نحوی مشخص.

---

# 29. نام نمی‌تواند با عدد شروع شود

غلط:

```js
let 1name = "Sina";
```

درست:

```js
let name1 = "Sina";
```

پس:

```text
name1 → درست
1name  → غلط
```

---

# 30. `_` مجاز است

```js
let _name = "Sina";
let _age = 23;
```

هر دو از نظر Syntax معتبر هستند.

---

# 31. `$` مجاز است

```js
let $price = 100;
let $button = document.querySelector("button");
```

`$` بخشی از Identifier مجاز JavaScript است.

البته استفاده از آن ممکن است در یک پروژه Convention خاصی داشته باشد.

---

# 32. فاصله مجاز نیست

غلط:

```js
let first name = "Sina";
```

درست:

```js
let firstName = "Sina";
```

یا:

```js
let first_name = "Sina";
```

---

# 33. خط تیره معمولی مجاز نیست

غلط:

```js
let first-name = "Sina";
```

در JavaScript `-` عملگر Minus است.

درست:

```js
let firstName = "Sina";
```

---

# 34. JavaScript به Case حساس است

این‌ها سه Identifier متفاوت هستند:

```js
let age = 23;
let Age = 24;
let AGE = 25;
```

بنابراین:

```js
console.log(age);
console.log(Age);
console.log(AGE);
```

سه مقدار متفاوت دارند.

---

# 35. Keywordها را نمی‌توان به عنوان Variable Name معمولی استفاده کرد

مثلاً این‌ها معتبر نیستند:

```js
let let = 10;
let const = 20;
let class = 30;
```

چون `let`، `const` و `class` کلمات رزروشده/کلیدی زبان هستند.

نمونه‌های دیگر:

```text
if
for
function
return
switch
while
```

---

# 36. Identifier چیست؟

**Identifier** نامی است که برای شناسایی موجودیت‌های برنامه استفاده می‌شود.

مثلاً:

```js
let age = 23;
```

`age` یک Identifier است.

Identifier می‌تواند نام:

```text
Variable
Function
Class
Parameter
```

باشد.

---

# 37. Naming Rule در برابر Naming Convention

این دو را قاطی نکن.

### Naming Rule

قانون خود زبان است.

مثلاً:

```js
let 1name = "Sina";
```

غلط است.

### Naming Convention

سبک نام‌گذاری پیشنهادی تیم یا پروژه است.

مثلاً:

```js
let firstName = "Sina";
```

و:

```js
let first_name = "Sina";
```

ممکن است هر دو از نظر Syntax صحیح باشند، ولی Convention متفاوتی دارند.

---

# 38. camelCase

در JavaScript برای Variable و Function معمولاً `camelCase` رایج است.

```js
let firstName = "Sina";
let userAge = 23;
let totalScore = 100;
```

ساختار:

```text
firstName
userAge
totalScore
```

کلمه اول با حرف کوچک شروع می‌شود و کلمات بعدی با حرف بزرگ.

---

# 39. PascalCase

در `PascalCase` هر کلمه با حرف بزرگ شروع می‌شود:

```js
UserProfile
ShoppingCart
MusicPlayer
```

در JavaScript معمولاً برای Classها استفاده می‌شود:

```js
class UserProfile {
}
```

و در بسیاری از Frameworkها برای Componentها نیز رایج است.

برای Variable معمولی:

```js
let userProfile;
```

معمولاً camelCase بهتر است.

---

# 40. snake_case

در این سبک از `_` استفاده می‌شود:

```js
let first_name = "Sina";
let user_age = 23;
```

از نظر Syntax معتبر است، اما در JavaScript معمولاً camelCase برای Variableهای معمولی رایج‌تر است.

---

# 41. انتخاب نام خوب

نام:

```js
let x = 500;
```

ممکن است معتبر باشد، اما معنی آن مشخص نیست.

بهتر:

```js
let productPrice = 500;
```

یا در یک بازی:

```js
let playerScore = 500;
```

نام خوب باید به Context کمک کند.

---

# 42. Boolean Naming

برای Booleanها معمولاً نامی انتخاب می‌کنیم که حالت یا سؤال بله/خیر را مشخص کند:

```js
let isLoggedIn = true;
let hasPermission = false;
let canEdit = true;
```

سپس:

```js
if (isLoggedIn) {
    // ...
}
```

خواناتر از:

```js
if (x) {
    // ...
}
```

است.

---

# 43. `const` یا `let`؟

یک Convention رایج در JavaScript مدرن:

> ابتدا `const` را انتخاب کن؛ اگر Variable نیاز به Reassignment داشت، `let` استفاده کن.

مثلاً:

```js
const username = "Sina";
const birthYear = 1380;
```

اگر مقدار باید تغییر کند:

```js
let score = 0;

score += 10;
```

---

# 44. آیا `var` را استفاده کنیم؟

برای کد جدید JavaScript معمولاً انتخاب اصلی:

```text
const
let
```

است.

`var` همچنان مهم است چون:

- باید کدهای قدیمی را بخوانی.
- رفتار Scope متفاوتی دارد.
- Hoisting آن با `let` و `const` متفاوت است.

بنابراین:

```text
یادگیری var → ضروری
استفاده از var در کد جدید → معمولاً انتخاب اول نیست
```

---

# 45. مثال کامل همه مفاهیم

```js
const appName = "Sudoku";

let score = 0;

score = 10;

var oldVersion = "1.0";

oldVersion = "2.0";
```

تحلیل:

```text
const appName = "Sudoku"
→ Declaration + Initialization
→ Reassignment ممنوع

let score = 0
→ Declaration + Initialization

score = 10
→ Reassignment

var oldVersion = "1.0"
→ Declaration + Initialization

oldVersion = "2.0"
→ Reassignment
```

---

# 46. اشتباهات رایج

### اشتباه 1

```js
const age;
```

❌ `const` باید Initialization داشته باشد.

### اشتباه 2

```js
const age = 23;
age = 24;
```

❌ Reassignment به `const` ممنوع است.

### اشتباه 3

```js
let 1user = "Sina";
```

❌ Identifier نمی‌تواند با عدد شروع شود.

### اشتباه 4

```js
let first-name = "Sina";
```

❌ `-` برای Identifier معمولی مجاز نیست.

### اشتباه 5

```js
let user name = "Sina";
```

❌ فاصله مجاز نیست.

### اشتباه 6

```js
let age = 23;
let age = 24;
```

❌ Redeclaration در همان Scope برای `let` مجاز نیست.

اما:

```js
let age = 23;
age = 24;
```

✅ Reassignment مجاز است.

---

# 47. ارتباط با جزوه‌های قبلی

برای فهم عمیق‌تر این مفاهیم:

- **Scope و Execution Context** به مبحث اجرای JavaScript مربوط است.
- **Call Stack و Stack Frame** در جزوه JavaScript Engine & Memory Management توضیح داده شده‌اند.
- **Heap و Reference** برای درک رفتار `const` با Object و Array مهم‌اند.
- **Event Loop و Async/Await** در جزوه Event Loop توضیح داده شده‌اند.

بنابراین Variable فقط یک «جعبه ساده» نیست؛ Variable یک **Binding** در محیط اجرای JavaScript است که به یک مقدار یا Object دسترسی می‌دهد.

---

# 48. Cheat Sheet

```text
DECLARE
↓
ایجاد/ثبت Variable Binding

INITIALIZE
↓
اولین مقداردهی

ASSIGN
↓
نسبت دادن مقدار

REASSIGN
↓
تغییر مقدار Binding موجود

REDECLARE
↓
Declare کردن دوباره در Scope مشابه
```

### `var`

```text
Function Scoped
Reassignable
Redeclarable در همان Scope
رفتار Hoisting متفاوت
```

### `let`

```text
Block Scoped
Reassignable
Not Redeclarable در همان Scope
TDZ
```

### `const`

```text
Block Scoped
Not Reassignable
Not Redeclarable در همان Scope
Must initialize
TDZ
```

---

# 49. Naming Rules Cheat Sheet

معتبر:

```js
userName
user_name
$price
_value
value2
```

نامعتبر:

```js
2value
user name
user-name
let
const
```

و:

```text
JavaScript → Case Sensitive
```

بنابراین:

```text
userName
UserName
USERNAME
```

سه Identifier متفاوت هستند.

---

# 50. جمع‌بندی نهایی

اگر فقط چند نکته را بخواهی حفظ کنی:

```text
Variable
→ نامی برای دسترسی به یک Binding/Value

Declaration
→ معرفی Variable

Initialization
→ اولین مقداردهی

Assignment
→ نسبت دادن مقدار

Reassignment
→ تغییر مقدار Variable موجود

Redeclaration
→ تعریف دوباره همان Variable در Scope مشابه
```

و:

```text
const → پیش‌فرض وقتی Reassignment لازم نیست

let   → وقتی مقدار باید تغییر کند

var   → بیشتر برای Legacy Code و درک JavaScript قدیمی
```

برای Naming:

```text
camelCase  → Variable / Function رایج
PascalCase → Class / Component رایج
snake_case → معتبر ولی Convention متفاوت
```

و مهم‌ترین تفاوت سه Keyword:

```text
var
└── Function Scope

let
└── Block Scope + Reassign

const
└── Block Scope + No Reassign
```
