## Created by AI


# جزوه JavaScript — Data Types، String، Number، Boolean و Symbol

## فهرست مطالب

1. Data Types
2. Primitive Values
3. String
4. `console.log()`
5. Index
6. `.length`
7. `charAt()`
8. `at()`
9. `indexOf()`
10. `lastIndexOf()`
11. `search()`
12. `includes()`
13. `substring()`
14. `slice()`
15. `startsWith()`
16. `endsWith()`
17. `trim()`
18. `trimStart()`
19. `trimEnd()`
20. `padStart()`
21. `padEnd()`
22. `replace()`
23. `replaceAll()`
24. `concat()`
25. Number
26. `toFixed()`
27. `Number.MAX_VALUE`
28. `Number.MIN_VALUE`
29. `2 / 0`
30. `'a' / 2`
31. `NaN`
32. Boolean
33. `Boolean()`
34. `String()`
35. `Number()`
36. Type Conversion
37. Type Coercion
38. `===`
39. `undefined`
40. `typeof`
41. Symbol
42. `null`
43. BigInt
44. جمع‌بندی و Cheat Sheet

---

# 1. Data Type چیست؟

**Data Type** یا «نوع داده» مشخص می‌کند یک مقدار در JavaScript چه نوعی است و چه رفتارهایی دارد.

مثلاً:

```javascript
let name = "Sina";
let age = 23;
let isDeveloper = true;
```

سه Value مختلف داریم:

```text
"Sina" → String

23 → Number

true → Boolean
```

بنابراین:

```text
Variable
   ↓
Value
   ↓
Data Type
```

---

# 2. انواع Data Type در JavaScript

به‌صورت کلی Data Typeهای JavaScript را می‌توان به دو گروه تقسیم کرد:

### Primitive

```text
String
Number
BigInt
Boolean
Undefined
Null
Symbol
```

### Object

```text
Object
Array
Function
Date
...
```

در این جزوه تمرکز اصلی روی این چهار نوع است:

```text
String
Number
Boolean
Symbol
```

ولی برای کامل شدن مبحث، `undefined`، `null` و `BigInt` را نیز بررسی می‌کنیم.

---

# 3. Primitive Value چیست؟

Primitiveها ساده‌ترین نوع Value در JavaScript هستند.

Primitive Data Types:

```text
String
Number
BigInt
Boolean
Undefined
Null
Symbol
```

مثلاً:

```javascript
let name = "Sina";
let age = 23;
let active = true;
```

هر سه Value از نوع Primitive هستند.

---

# 4. String

`String` برای نگهداری **متن** استفاده می‌شود.

```javascript
let name = "Sina";
```

در اینجا:

```text
Sina
```

یک String است.

---

## روش‌های ایجاد String

### Double Quote

```javascript
let name = "Sina";
```

### Single Quote

```javascript
let name = 'Sina';
```

### Backtick

```javascript
let name = `Sina`;
```

هر سه String تولید می‌کنند.

---

# 5. Template Literal

Backtick علاوه بر ساخت String معمولی، امکان قرار دادن Expression داخل String را فراهم می‌کند.

```javascript
let name = "Sina";
let age = 23;

console.log(`My name is ${name} and I am ${age}.`);
```

خروجی:

```text
My name is Sina and I am 23.
```

این قابلیت را **Template Literal** می‌نامیم.

---

# 6. String و Number یکی نیستند

این دو را دقت کن:

```javascript
let a = 123;
let b = "123";
```

نوعشان:

```text
a → Number

b → String
```

حتی اگر ظاهرشان مشابه باشد.

مثلاً:

```javascript
console.log(123 + 1);
```

نتیجه:

```text
124
```

اما:

```javascript
console.log("123" + 1);
```

نتیجه:

```text
1231
```

چون در دومی با String سروکار داریم.

---

# 7. `console.log()`

`console.log()` برای نمایش اطلاعات در Console استفاده می‌شود.

```javascript
console.log("Hello");
```

یا:

```javascript
let age = 23;

console.log(age);
```

خروجی:

```text
23
```

می‌توان چند مقدار را همزمان نمایش داد:

```javascript
let name = "Sina";
let age = 23;

console.log(name, age);
```

در مرورگر می‌توان خروجی را در:

```text
DevTools → Console
```

دید.

---

# 8. Index چیست؟

هر کاراکتر داخل String یک موقعیت یا **Index** دارد.

نکته‌ی بسیار مهم:

> Index در JavaScript از `0` شروع می‌شود.

مثلاً:

```javascript
let word = "Hello";
```

ساختار:

```text
Character:

H   e   l   l   o
↓   ↓   ↓   ↓   ↓
0   1   2   3   4
```

بنابراین:

```javascript
word[0]
```

نتیجه:

```text
H
```

و:

```javascript
word[4]
```

نتیجه:

```text
o
```

---

# 9. Index و Value چه تفاوتی دارند؟

در:

```javascript
let word = "Hello";
```

داریم:

```text
H   e   l   l   o
0   1   2   3   4
```

عدد `2` یک **Index** است.

حرف `l` یک **Value** است.

بنابراین:

```javascript
word[2]
```

یعنی:

> Value موجود در Index شماره‌ی 2 را بده.

نتیجه:

```text
l
```

---

# 10. `.length`

`.length` طول String را مشخص می‌کند.

```javascript
let word = "Hello";

console.log(word.length);
```

نتیجه:

```text
5
```

چون پنج کاراکتر داریم:

```text
H e l l o
```

اما توجه کن:

```text
length = 5
last index = 4
```

پس:

```javascript
word[word.length - 1]
```

آخرین کاراکتر را می‌دهد:

```text
o
```

---

# 11. `charAt()`

`charAt()` کاراکتر موجود در یک Index را برمی‌گرداند.

```javascript
let word = "Hello";

console.log(word.charAt(1));
```

نتیجه:

```text
e
```

مثال:

```javascript
word.charAt(0); // H
word.charAt(2); // l
word.charAt(4); // o
```

اگر Index خارج از محدوده باشد:

```javascript
word.charAt(10);
```

نتیجه:

```text
""
```

یعنی String خالی.

---

# 12. `at()`

`at()` نیز برای دسترسی به Value موجود در یک Index استفاده می‌شود.

```javascript
let word = "Hello";

console.log(word.at(1));
```

نتیجه:

```text
e
```

اما یک قابلیت مهم دارد:

> `at()` از Index منفی پشتیبانی می‌کند.

```javascript
console.log(word.at(-1));
```

نتیجه:

```text
o
```

مثال:

```javascript
word.at(-1); // o
word.at(-2); // l
word.at(-3); // l
```

ساختار:

```text
H   e   l   l   o
0   1   2   3   4
-5 -4  -3  -2  -1
```

---

# 13. `indexOf()`

`indexOf()` اولین occurrence یک Value را پیدا می‌کند.

```javascript
let word = "Hello";

console.log(word.indexOf("l"));
```

نتیجه:

```text
2
```

چرا؟

```text
H e l l o
0 1 2 3 4
    ↑
```

اولین `l` در Index شماره‌ی `2` است.

اگر پیدا نشود:

```javascript
console.log(word.indexOf("x"));
```

نتیجه:

```text
-1
```

پس:

```text
پیدا شد → Index

پیدا نشد → -1
```

---

# 14. `lastIndexOf()`

`lastIndexOf()` آخرین occurrence یک Value را پیدا می‌کند.

```javascript
let word = "Hello";

console.log(word.lastIndexOf("l"));
```

نتیجه:

```text
3
```

چون:

```text
H e l l o
0 1 2 3 4
      ↑
```

مقایسه:

```javascript
word.indexOf("l");     // 2
word.lastIndexOf("l"); // 3
```

---

# 15. `search()`

`search()` برای جستجو داخل String استفاده می‌شود.

```javascript
let text = "Hello World";

console.log(text.search("World"));
```

نتیجه:

```text
6
```

یکی از کاربردهای مهم `search()` استفاده از Regular Expression است.

مثلاً:

```javascript
let text = "Hello 123";

console.log(text.search(/[0-9]/));
```

نتیجه:

```text
6
```

چون اولین رقم در Index شماره‌ی `6` قرار دارد.

---

# 16. `includes()`

`includes()` بررسی می‌کند آیا یک مقدار داخل String وجود دارد یا خیر.

نتیجه:

```text
true
```

یا:

```text
false
```

مثال:

```javascript
let text = "Hello World";

console.log(text.includes("World"));
```

نتیجه:

```text
true
```

اما:

```javascript
console.log(text.includes("Sina"));
```

نتیجه:

```text
false
```

تفاوت:

```text
indexOf()
    → موقعیت را می‌دهد

includes()
    → وجود یا عدم وجود را می‌دهد
```

---

# 17. `substring()`

`substring()` بخشی از String را استخراج می‌کند.

```javascript
let text = "JavaScript";

console.log(text.substring(0, 4));
```

نتیجه:

```text
Java
```

ساختار:

```javascript
substring(start, end)
```

نکته:

`end` جزو نتیجه نیست.

یعنی:

```javascript
text.substring(0, 4)
```

Indexهای:

```text
0
1
2
3
```

را می‌گیرد.

---

# 18. `slice()`

`slice()` نیز بخشی از String را استخراج می‌کند.

```javascript
let text = "JavaScript";

console.log(text.slice(0, 4));
```

نتیجه:

```text
Java
```

ساختار:

```javascript
slice(start, end)
```

باز هم `end` شامل نتیجه نمی‌شود.

---

# 19. تفاوت `substring()` و `slice()`

یکی از تفاوت‌های مهم، رفتار با Index منفی است.

`slice()` از Index منفی پشتیبانی می‌کند:

```javascript
let text = "JavaScript";

console.log(text.slice(-6));
```

نتیجه:

```text
Script
```

اما:

```javascript
text.substring(-6);
```

مقدار منفی را مانند `0` در نظر می‌گیرد.

بنابراین برای کار با Index منفی:

```javascript
slice()
```

گزینه‌ی مناسب‌تری است.

---

# 20. `startsWith()`

بررسی می‌کند String با مقدار مشخصی شروع می‌شود یا خیر.

```javascript
let text = "JavaScript";

console.log(text.startsWith("Java"));
```

نتیجه:

```text
true
```

اما:

```javascript
console.log(text.startsWith("Script"));
```

نتیجه:

```text
false
```

---

# 21. `endsWith()`

بررسی می‌کند String با مقدار مشخصی تمام می‌شود یا خیر.

```javascript
let file = "photo.jpg";

console.log(file.endsWith(".jpg"));
```

نتیجه:

```text
true
```

مثلاً:

```javascript
console.log(file.endsWith(".png"));
```

نتیجه:

```text
false
```

---

# 22. `trim()`

`trim()` فاصله‌های خالی ابتدای و انتهای String را حذف می‌کند.

```javascript
let text = "   Hello World   ";

console.log(text.trim());
```

نتیجه:

```text
Hello World
```

اما فاصله‌های وسط را حذف نمی‌کند:

```javascript
"Hello   World".trim();
```

نتیجه:

```text
Hello   World
```

---

# 23. `trimStart()`

فقط فاصله‌های ابتدای String را حذف می‌کند.

```javascript
let text = "   Hello";

console.log(text.trimStart());
```

نتیجه:

```text
Hello
```

---

# 24. `trimEnd()`

فقط فاصله‌های انتهای String را حذف می‌کند.

```javascript
let text = "Hello   ";

console.log(text.trimEnd());
```

نتیجه:

```text
Hello
```

---

# 25. `padStart()`

`padStart()` از ابتدای String کاراکتر اضافه می‌کند تا String به طول مشخصی برسد.

```javascript
let number = "5";

console.log(number.padStart(3, "0"));
```

نتیجه:

```text
005
```

مثال کاربردی:

```javascript
let code = "42";

console.log(code.padStart(5, "0"));
```

نتیجه:

```text
00042
```

---

# 26. `padEnd()`

`padEnd()` از انتهای String کاراکتر اضافه می‌کند.

```javascript
let number = "5";

console.log(number.padEnd(3, "0"));
```

نتیجه:

```text
500
```

---

# 27. `replace()`

`replace()` یک بخش از String را با مقدار دیگری جایگزین می‌کند.

```javascript
let text = "Hello World";

console.log(text.replace("World", "Sina"));
```

نتیجه:

```text
Hello Sina
```

اما نکته مهم:

در حالت معمول فقط **اولین occurrence** را جایگزین می‌کند.

```javascript
let text = "cat cat cat";

console.log(text.replace("cat", "dog"));
```

نتیجه:

```text
dog cat cat
```

---

# 28. `replaceAll()`

`replaceAll()` همه occurrenceها را جایگزین می‌کند.

```javascript
let text = "cat cat cat";

console.log(text.replaceAll("cat", "dog"));
```

نتیجه:

```text
dog dog dog
```

پس:

```text
replace()
    اولین occurrence

replaceAll()
    همه occurrenceها
```

---

# 29. `concat()`

`concat()` برای اتصال Stringها استفاده می‌شود.

```javascript
let firstName = "Sina";
let lastName = "Saeidei";

console.log(firstName.concat(" ", lastName));
```

نتیجه:

```text
Sina Saeidei
```

البته در JavaScript مدرن، Template Literal معمولاً خواناتر است:

```javascript
console.log(`${firstName} ${lastName}`);
```

---

# 30. Stringها Immutable هستند

String یک Primitive Value است و نمی‌توان محتوای خود String را مستقیماً تغییر داد.

متدهایی مثل:

```javascript
trim()
replace()
toUpperCase()
slice()
```

معمولاً یک String جدید برمی‌گردانند.

مثال:

```javascript
let text = "Hello";

text.toUpperCase();

console.log(text);
```

هنوز:

```text
Hello
```

است.

اگر نتیجه را بخواهیم نگه داریم:

```javascript
text = text.toUpperCase();
```

اکنون:

```text
HELLO
```

داریم.

این موضوع به مبحث Variable و Assignment جزوه قبلی نیز مربوط است.

---

# 31. Number

`Number` برای اعداد معمولی JavaScript استفاده می‌شود.

مثال:

```javascript
let age = 23;
let price = 19.99;
let temperature = -5;
```

همه‌ی این‌ها:

```text
Number
```

هستند.

حتی:

```javascript
10
10.5
-20
0
```

همگی `Number` هستند.

---

# 32. Integer و Decimal

در JavaScript معمولاً نوع جداگانه‌ای برای Integer و Decimal مانند برخی زبان‌ها نداریم.

```javascript
let a = 10;
let b = 10.5;

console.log(typeof a);
console.log(typeof b);
```

هر دو:

```text
number
```

هستند.

---

# 33. `toFixed()`

`toFixed()` برای نمایش یک Number با تعداد مشخصی رقم اعشار استفاده می‌شود.

```javascript
let price = 12.34567;

console.log(price.toFixed(2));
```

نتیجه:

```text
"12.35"
```

### نکته بسیار مهم

`toFixed()` یک **String** برمی‌گرداند، نه Number.

```javascript
let result = (12.34567).toFixed(2);

console.log(typeof result);
```

نتیجه:

```text
string
```

بنابراین:

```text
toFixed()
    Number → String
```

---

# 34. کاربرد `toFixed()`

مثلاً قیمت:

```javascript
let price = 19.999;

console.log(price.toFixed(2));
```

نتیجه:

```text
"20.00"
```

پس `toFixed()` بیشتر برای **قالب‌بندی و نمایش عدد** استفاده می‌شود.

---

# 35. `Number.MAX_VALUE`

`Number.MAX_VALUE` بزرگ‌ترین مقدار مثبت **finite** قابل نمایش توسط `Number` است.

```javascript
console.log(Number.MAX_VALUE);
```

تقریباً:

```text
1.7976931348623157e+308
```

است.

---

# 36. `Number.MIN_VALUE`

اینجا یک نکته‌ی بسیار مهم وجود دارد.

ممکن است تصور کنیم:

```javascript
Number.MIN_VALUE
```

یعنی:

> کوچک‌ترین عدد

یا:

> بزرگ‌ترین عدد منفی

اما این درست نیست.

`Number.MIN_VALUE` یعنی:

> کوچک‌ترین مقدار **مثبت و غیرصفر** قابل نمایش با Number.

تقریباً:

```text
5e-324
```

است.

بنابراین:

```text
MAX_VALUE
    بزرگ‌ترین Number مثبت finite

MIN_VALUE
    کوچک‌ترین Number مثبت غیرصفر
```

---

# 37. `2 / 0`

در ریاضیات معمولی تقسیم بر صفر تعریف نشده است.

اما JavaScript برای Number از استاندارد IEEE 754 استفاده می‌کند.

بنابراین:

```javascript
console.log(2 / 0);
```

نتیجه:

```text
Infinity
```

است.

و:

```javascript
console.log(-2 / 0);
```

نتیجه:

```text
-Infinity
```

است.

---

# 38. Infinity

`Infinity` یک مقدار ویژه‌ی Number است.

```javascript
console.log(Infinity);
```

و:

```javascript
console.log(typeof Infinity);
```

نتیجه:

```text
number
```

است.

یعنی:

```javascript
typeof Infinity
```

برابر است با:

```text
"number"
```

---

# 39. `'a' / 2`

حالا این را ببین:

```javascript
console.log("a" / 2);
```

JavaScript تلاش می‌کند `"a"` را به Number تبدیل کند.

اما:

```text
"a"
```

قابل تبدیل به یک Number معتبر نیست.

در نتیجه:

```text
NaN
```

تولید می‌شود.

---

# 40. NaN

`NaN` مخفف:

> Not-a-Number

است.

مثال:

```javascript
console.log("hello" / 2);
```

نتیجه:

```text
NaN
```

یا:

```javascript
console.log(Number("abc"));
```

نتیجه:

```text
NaN
```

### نکته عجیب

```javascript
console.log(typeof NaN);
```

نتیجه:

```text
"number"
```

است.

یعنی با وجود نام `Not-a-Number`، Type آن در JavaScript برابر `number` است.

---

# 41. Boolean

Boolean فقط دو Value دارد:

```javascript
true
false
```

مثال:

```javascript
let isLoggedIn = true;
let isAdmin = false;
```

Boolean برای وضعیت‌ها و شرط‌ها بسیار مهم است.

مثال:

```javascript
let age = 23;

console.log(age >= 18);
```

نتیجه:

```text
true
```

---

# 42. Truthy و Falsy

JavaScript در Contextهای Boolean می‌تواند بعضی Valueها را به `true` یا `false` ارزیابی کند.

مقادیر مهم Falsy:

```text
false
0
-0
0n
""
null
undefined
NaN
```

مثلاً:

```javascript
if ("Hello") {
    console.log("Run");
}
```

اجرا می‌شود، چون:

```text
"Hello"
```

Truthy است.

اما:

```javascript
if ("") {
    console.log("Run");
}
```

اجرا نمی‌شود، چون String خالی Falsy است.

---

# 43. `Boolean()`

`Boolean()` یک Value را به Boolean تبدیل می‌کند.

```javascript
console.log(Boolean(1));
```

نتیجه:

```text
true
```

اما:

```javascript
console.log(Boolean(0));
```

نتیجه:

```text
false
```

مثال‌های مهم:

```javascript
Boolean("Hello");   // true
Boolean("");        // false
Boolean(123);       // true
Boolean(0);         // false
Boolean(null);      // false
Boolean(undefined); // false
```

---

# 44. `Number()`

`Number()` یک Value را به Number تبدیل می‌کند.

```javascript
console.log(Number("123"));
```

نتیجه:

```text
123
```

مثال:

```javascript
Number("3.14"); // 3.14
Number("100");  // 100
```

اما:

```javascript
Number("Hello");
```

نتیجه:

```text
NaN
```

---

# 45. `String()`

`String()` یک Value را به String تبدیل می‌کند.

```javascript
console.log(String(123));
```

نتیجه:

```text
"123"
```

مثال:

```javascript
String(true);      // "true"
String(false);     // "false"
String(123);       // "123"
String(null);      // "null"
String(undefined); // "undefined"
```

---

# 46. Type Conversion

وقتی خودمان صریحاً Type را تغییر می‌دهیم، **Type Conversion** انجام داده‌ایم.

مثلاً:

```javascript
let value = "123";

value = Number(value);
```

ابتدا:

```text
"123"
↓
String
```

و سپس:

```text
123
↓
Number
```

توابع اصلی:

```javascript
String()
Number()
Boolean()
```

هستند.

---

# 47. Type Coercion

گاهی JavaScript بدون اینکه خودمان تابع Conversion را صدا بزنیم، Type را تبدیل می‌کند.

مثلاً:

```javascript
console.log("5" + 2);
```

نتیجه:

```text
"52"
```

اما:

```javascript
console.log("5" - 2);
```

نتیجه:

```text
3
```

این تبدیل خودکار را **Type Coercion** می‌نامیم.

---

# 48. `===`

`===` عملگر **Strict Equality** است.

یعنی هم Value و هم Type را بررسی می‌کند.

```javascript
console.log(5 === 5);
```

نتیجه:

```text
true
```

اما:

```javascript
console.log(5 === "5");
```

نتیجه:

```text
false
```

چون:

```text
5   → Number
"5" → String
```

---

# 49. تفاوت `===` و `==`

این دو را با هم اشتباه نکن:

```javascript
==
===
```

مثال:

```javascript
console.log(5 == "5");
```

نتیجه:

```text
true
```

اما:

```javascript
console.log(5 === "5");
```

نتیجه:

```text
false
```

دلیل:

```text
== 
    ممکن است Type Conversion انجام دهد

===
    Type را نیز بررسی می‌کند
```

در کدنویسی مدرن JavaScript معمولاً `===` رفتار قابل‌پیش‌بینی‌تری دارد.

---

# 50. `undefined`

`undefined` یکی از Primitive Data Typeهای JavaScript است.

اگر Variable را Declare کنیم اما مقداردهی نکنیم:

```javascript
let name;
```

مقدار آن:

```text
undefined
```

است.

```javascript
console.log(name);
```

نتیجه:

```text
undefined
```

و:

```javascript
console.log(typeof name);
```

نتیجه:

```text
"undefined"
```

---

# 51. `typeof`

`typeof` برای بررسی Type یک Value استفاده می‌شود.

مثال:

```javascript
typeof "Hello"
```

نتیجه:

```text
"string"
```

---

```javascript
typeof 123
```

نتیجه:

```text
"number"
```

---

```javascript
typeof true
```

نتیجه:

```text
"boolean"
```

---

```javascript
typeof undefined
```

نتیجه:

```text
"undefined"
```

---

```javascript
typeof Symbol()
```

نتیجه:

```text
"symbol"
```

---

# 52. یک نکته معروف درباره `typeof null`

این مورد یکی از رفتارهای تاریخی JavaScript است:

```javascript
console.log(typeof null);
```

نتیجه:

```text
"object"
```

اما `null` در واقع یک Primitive Value است.

بنابراین:

```text
typeof null
```

یک استثنای تاریخی و عجیب در JavaScript محسوب می‌شود.

---

# 53. Symbol

`Symbol` یکی از Primitive Data Typeهای JavaScript است.

Symbol برای ایجاد Valueهای **منحصربه‌فرد** استفاده می‌شود.

```javascript
const id = Symbol();
```

هر بار که `Symbol()` اجرا می‌شود، Symbol جدیدی ساخته می‌شود.

```javascript
const a = Symbol();
const b = Symbol();

console.log(a === b);
```

نتیجه:

```text
false
```

---

# 54. Symbol با Description

می‌توان برای Symbol یک Description قرار داد:

```javascript
const id = Symbol("id");
```

اما Description باعث نمی‌شود دو Symbol یکسان شوند:

```javascript
const a = Symbol("id");
const b = Symbol("id");

console.log(a === b);
```

نتیجه:

```text
false
```

چون:

```text
a ≠ b
```

---

# 55. کاربرد Symbol

یکی از کاربردهای مهم Symbol استفاده به عنوان Property Key در Object است.

```javascript
const id = Symbol("id");

const user = {
    name: "Sina",
    [id]: 123
};
```

در اینجا:

```javascript
id
```

یک Symbol است.

Symbol در مباحث پیشرفته‌تر Object، Property و Meta-programming اهمیت بیشتری پیدا می‌کند.

---

# 56. BigInt

یکی دیگر از Primitive Data Typeها:

```text
BigInt
```

است.

برای Integerهای بسیار بزرگ استفاده می‌شود.

مثال:

```javascript
const bigNumber = 9007199254740991n;
```

حرف:

```text
n
```

در انتهای عدد مشخص می‌کند که Value از نوع BigInt است.

```javascript
console.log(typeof bigNumber);
```

نتیجه:

```text
"bigint"
```

---

# 57. `Number.MAX_VALUE` با `Number.MAX_SAFE_INTEGER` فرق دارد

این دو را نباید یکی بدانیم.

### `Number.MAX_VALUE`

بزرگ‌ترین Number مثبت finite:

```javascript
Number.MAX_VALUE
```

تقریباً:

```text
1.7976931348623157e+308
```

### `Number.MAX_SAFE_INTEGER`

بزرگ‌ترین Integer مثبتی که عملیات صحیح معمول روی آن با دقت امن انجام می‌شود:

```javascript
Number.MAX_SAFE_INTEGER
```

برابر:

```text
9007199254740991
```

است.

بنابراین:

```text
MAX_VALUE
    مربوط به محدوده کلی Number

MAX_SAFE_INTEGER
    مربوط به دقت Integerها
```

است.

---

# 58. Unicode و `.length`

یک نکته مهم‌تر درباره String:

JavaScript Stringها را بر اساس **UTF-16 code units** مدیریت می‌کند.

بنابراین `.length` همیشه دقیقاً برابر با تعداد کاراکترهایی که انسان روی صفحه می‌بیند نیست.

مثلاً:

```javascript
console.log("😀".length);
```

نتیجه:

```text
2
```

است.

دلیل این است که این Emoji از دو UTF-16 code unit تشکیل شده است.

پس دقیق‌تر است بگوییم:

```text
String.length
    تعداد UTF-16 code unitها
```

را گزارش می‌کند.

این مسئله در کار با Unicode، Emoji و پردازش متن فارسی در پروژه‌های بزرگ‌تر اهمیت پیدا می‌کند.

---

# 59. یک مثال ترکیبی String

فرض کنیم Username کاربر این باشد:

```javascript
let username = "   Sina123   ";
```

ابتدا فاصله‌ها را حذف می‌کنیم:

```javascript
username = username.trim();
```

حالا:

```text
Sina123
```

بررسی می‌کنیم با `Sina` شروع می‌شود:

```javascript
console.log(username.startsWith("Sina"));
```

نتیجه:

```text
true
```

بررسی می‌کنیم `123` داخل آن هست:

```javascript
console.log(username.includes("123"));
```

نتیجه:

```text
true
```

طول:

```javascript
console.log(username.length);
```

آخرین کاراکتر:

```javascript
console.log(username.at(-1));
```

نتیجه:

```text
3
```

---

# 60. یک مثال ترکیبی Number

```javascript
let price = "19.999";
```

تبدیل:

```javascript
price = Number(price);
```

اکنون:

```text
"19.999"
↓
19.999
```

Type:

```javascript
console.log(typeof price);
```

نتیجه:

```text
number
```

قالب‌بندی:

```javascript
console.log(price.toFixed(2));
```

نتیجه:

```text
"20.00"
```

بنابراین:

```text
Number()
    تبدیل Type

toFixed()
    قالب‌بندی نمایش Number
```

---

# 61. یک مثال ترکیبی Boolean

```javascript
let username = "Sina";

console.log(Boolean(username));
```

چون String خالی نیست:

```text
true
```

اما:

```javascript
let username = "";

console.log(Boolean(username));
```

نتیجه:

```text
false
```

---

# 62. یک مثال ترکیبی `===`

```javascript
let age = "23";

console.log(age === 23);
```

نتیجه:

```text
false
```

چون:

```text
"23" → String

23 → Number
```

اگر تبدیل کنیم:

```javascript
console.log(Number(age) === 23);
```

نتیجه:

```text
true
```

---

# 63. جدول متدهای String

| Method / Property | کاربرد                                    |
| ----------------- | ----------------------------------------- |
| `.length`         | طول String بر اساس UTF-16 code unit       |
| `charAt()`        | گرفتن کاراکتر با Index                    |
| `at()`            | گرفتن کاراکتر با Index و پشتیبانی از منفی |
| `indexOf()`       | پیدا کردن اولین occurrence                |
| `lastIndexOf()`   | پیدا کردن آخرین occurrence                |
| `search()`        | جستجو با String یا RegExp                 |
| `includes()`      | بررسی وجود مقدار                          |
| `substring()`     | استخراج بخشی از String                    |
| `slice()`         | استخراج بخشی از String                    |
| `startsWith()`    | بررسی ابتدای String                       |
| `endsWith()`      | بررسی انتهای String                       |
| `trim()`          | حذف فضای ابتدا و انتها                    |
| `trimStart()`     | حذف فضای ابتدا                            |
| `trimEnd()`       | حذف فضای انتها                            |
| `padStart()`      | افزودن Padding از ابتدا                   |
| `padEnd()`        | افزودن Padding از انتها                   |
| `replace()`       | جایگزینی occurrence                       |
| `replaceAll()`    | جایگزینی همه occurrenceها                 |
| `concat()`        | اتصال Stringها                            |

---

# 64. جدول تبدیل Type

| Function    | مثال            | نتیجه   |
| ----------- | --------------- | ------- |
| `String()`  | `String(123)`   | `"123"` |
| `Number()`  | `Number("123")` | `123`   |
| `Boolean()` | `Boolean(1)`    | `true`  |

---

# 65. جدول مقادیر ویژه Number

| Value                     | مفهوم                          |
| ------------------------- | ------------------------------ |
| `Infinity`                | بی‌نهایت مثبت                  |
| `-Infinity`               | بی‌نهایت منفی                  |
| `NaN`                     | نتیجه‌ی نامعتبر یک عملیات عددی |
| `Number.MAX_VALUE`        | بزرگ‌ترین Number مثبت finite   |
| `Number.MIN_VALUE`        | کوچک‌ترین Number مثبت غیرصفر   |
| `Number.MAX_SAFE_INTEGER` | بزرگ‌ترین Integer مثبت امن     |

---

# 66. خطاهای رایج

### اشتباه ۱ — اشتباه گرفتن `length` با آخرین Index

```javascript
let word = "Hello";

console.log(word.length); // 5
console.log(word[5]);     // undefined
```

آخرین Index:

```javascript
word.length - 1
```

است.

---

### اشتباه ۲ — تصور اینکه `toFixed()` Number برمی‌گرداند

```javascript
let result = (12.345).toFixed(2);

console.log(typeof result);
```

نتیجه:

```text
string
```

---

### اشتباه ۳ — تصور اینکه `replace()` همه موارد را تغییر می‌دهد

```javascript
"cat cat".replace("cat", "dog");
```

نتیجه:

```text
"dog cat"
```

برای همه:

```javascript
"cat cat".replaceAll("cat", "dog");
```

---

### اشتباه ۴ — تصور اینکه `Number.MIN_VALUE` منفی است

```javascript
Number.MIN_VALUE
```

یک عدد **مثبت بسیار کوچک** است.

---

### اشتباه ۵ — تصور اینکه `NaN` یک Type مستقل است

```javascript
typeof NaN
```

نتیجه:

```text
"number"
```

---

### اشتباه ۶ — اشتباه گرفتن `===` و `==`

```javascript
5 == "5"   // true
5 === "5"  // false
```

---

# 67. ارتباط با جزوه Variables

در جزوه قبلی داشتیم:

```javascript
let age = 23;
```

این دستور را می‌توان این‌گونه تحلیل کرد:

```text
let
 ↓
Declaration

age
 ↓
Identifier

23
 ↓
Value

Number
 ↓
Data Type
```

پس:

```text
Variable
    ↓
Binding
    ↓
Value
    ↓
Data Type
```

این جزوه در واقع ادامه‌ی مستقیم مبحث Variables است.

---

# 68. ارتباط با Memory

در مباحث قبلی درباره‌ی:

```text
Heap
Call Stack
Memory
Reference
Garbage Collection
```

صحبت کردیم.

در اینجا با Primitiveهایی مثل:

```text
String
Number
Boolean
Symbol
Undefined
```

کار می‌کنیم.

برای Objectها و Arrayها باید مفهوم Reference و رفتار Objectها را جداگانه بررسی کنیم.

---

# 69. Cheat Sheet نهایی

## Data Types

```text
String
Number
BigInt
Boolean
Undefined
Null
Symbol
Object
```

---

## String

```javascript
"Hello"
```

---

## Number

```javascript
123
3.14
-20
```

---

## Boolean

```javascript
true
false
```

---

## Symbol

```javascript
Symbol("id")
```

---

## Index

```javascript
let text = "Hello";

text[0]; // H
text[1]; // e
```

---

## String Length

```javascript
text.length
```

---

## Character Access

```javascript
text.charAt(0)
text.at(0)
text.at(-1)
```

---

## Searching

```javascript
text.indexOf("l")
text.lastIndexOf("l")
text.search("l")
text.includes("Hello")
```

---

## Extracting

```javascript
text.substring(0, 3)
text.slice(0, 3)
```

---

## Beginning / Ending

```javascript
text.startsWith("He")
text.endsWith("lo")
```

---

## Whitespace

```javascript
text.trim()
text.trimStart()
text.trimEnd()
```

---

## Padding

```javascript
text.padStart(5, "0")
text.padEnd(5, "0")
```

---

## Replacement

```javascript
text.replace("a", "b")
text.replaceAll("a", "b")
```

---

## Concatenation

```javascript
text.concat("Hello")
```

---

## Number

```javascript
Number.MAX_VALUE
Number.MIN_VALUE
```

---

## Number Special Values

```text
Infinity
-Infinity
NaN
```

---

## Conversion

```javascript
String()
Number()
Boolean()
```

---

## Comparison

```javascript
===
```

---

## Type Checking

```javascript
typeof value
```

---

# 70. خلاصه نهایی

اگر بخواهیم کل این جزوه را در چند اصل خلاصه کنیم:

1. **Data Type** مشخص می‌کند یک Value چه نوعی است.
2. `String` برای متن است.
3. `Number` برای اعداد معمولی است.
4. `Boolean` فقط `true` و `false` دارد.
5. `Symbol` یک Primitive Value یکتا است.
6. Index در String از `0` شروع می‌شود.
7. `.length` تعداد UTF-16 code unitها را می‌دهد.
8. `charAt()` برای گرفتن کاراکتر با Index است.
9. `at()` همین کار را انجام می‌دهد و Index منفی را هم پشتیبانی می‌کند.
10. `indexOf()` اولین موقعیت را پیدا می‌کند.
11. `lastIndexOf()` آخرین موقعیت را پیدا می‌کند.
12. `search()` برای جستجو و مخصوصاً Regular Expression کاربرد دارد.
13. `includes()` فقط وجود یا عدم وجود را بررسی می‌کند.
14. `substring()` و `slice()` برای استخراج بخشی از String هستند.
15. `slice()` از Index منفی پشتیبانی می‌کند.
16. `startsWith()` شروع String را بررسی می‌کند.
17. `endsWith()` پایان String را بررسی می‌کند.
18. `trim()` فضای ابتدا و انتها را حذف می‌کند.
19. `trimStart()` فقط ابتدای String را حذف می‌کند.
20. `trimEnd()` فقط انتهای String را حذف می‌کند.
21. `padStart()` از ابتدای String Padding اضافه می‌کند.
22. `padEnd()` از انتهای String Padding اضافه می‌کند.
23. `replace()` معمولاً اولین occurrence را تغییر می‌دهد.
24. `replaceAll()` همه occurrenceها را تغییر می‌دهد.
25. `concat()` برای اتصال Stringهاست.
26. `toFixed()` برای قالب‌بندی Number است و نتیجه‌ی آن String است.
27. `Number.MAX_VALUE` بزرگ‌ترین Number مثبت finite است.
28. `Number.MIN_VALUE` کوچک‌ترین Number مثبت غیرصفر است.
29. `2 / 0` نتیجه‌ی `Infinity` می‌دهد.
30. `"a" / 2` نتیجه‌ی `NaN` می‌دهد.
31. `NaN` با وجود نامش، از نظر `typeof` یک `number` است.
32. `Boolean()` برای تبدیل Value به Boolean است.
33. `Number()` برای تبدیل Value به Number است.
34. `String()` برای تبدیل Value به String است.
35. `===` هم Value و هم Type را مقایسه می‌کند.
36. `undefined` یک Primitive Value است.
37. `typeof` برای بررسی Type استفاده می‌شود.
38. `typeof null` به دلیل یک رفتار تاریخی JavaScript برابر `"object"` است.
39. `Symbol()` هر بار یک Symbol یکتا تولید می‌کند.
40. `BigInt` برای Integerهای بسیار بزرگ کاربرد دارد.
41. Stringها Immutable هستند و متدهایشان معمولاً String جدید ایجاد می‌کنند.
42. **Type Conversion** تبدیل صریح Type است.
43. **Type Coercion** تبدیل خودکار Type توسط JavaScript است.