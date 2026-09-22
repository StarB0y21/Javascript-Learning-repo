## Created by AI


# جزوه JavaScript — Array و Object Methods

---

# 1. Array چیست؟

`Array` یا **آرایه** یکی از ساختارهای داده‌ای مهم JavaScript است که برای نگهداری چند مقدار در یک مجموعه استفاده می‌شود.

مثلاً:

```javascript
const fruits = ["Apple", "Banana", "Orange"];
```

در اینجا متغیر `fruits` یک Array دارد که سه مقدار درون آن قرار گرفته است.

می‌توانیم این مقادیر را با `index` پیدا کنیم:

```javascript
console.log(fruits[0]); // Apple
console.log(fruits[1]); // Banana
console.log(fruits[2]); // Orange
```

همان‌طور که در جزوه `String` دیدیم، **index از صفر شروع می‌شود**.

```text
Index:    0          1          2
          ↓          ↓          ↓
Array: ["Apple", "Banana", "Orange"]
```

---

# 2. Array می‌تواند چند نوع داده را نگهداری کند

برخلاف بعضی زبان‌ها، Array در JavaScript مجبور نیست فقط یک نوع داده داشته باشد.

مثلاً:

```javascript
const data = [
    "Sina",
    24,
    true,
    undefined,
    null
];
```

حتی می‌توانیم Object یا Array دیگری داخل Array قرار دهیم:

```javascript
const user = {
    name: "Sina",
    age: 24
};

const data = [
    "Hello",
    25,
    user,
    [1, 2, 3]
];
```

بنابراین Array می‌تواند ساختاری بسیار انعطاف‌پذیر داشته باشد.

---

# 3. `Array`

`Array` نام **constructor / built-in object** مربوط به آرایه‌ها در JavaScript است.

دو روش رایج برای ساخت Array داریم:

```javascript
const numbers = [1, 2, 3];
```

و:

```javascript
const numbers = new Array(1, 2, 3);
```

روش اول معمولاً ساده‌تر و خواناتر است.

---

## یک نکته مهم درباره `new Array()`

این دو با هم فرق دارند:

```javascript
const a = new Array(3);
```

و:

```javascript
const b = new Array(1, 2, 3);
```

اولی:

```javascript
[empty, empty, empty]
```

یعنی Array با طول 3 ایجاد می‌کند.

اما دومی:

```javascript
[1, 2, 3]
```

ایجاد می‌کند.

به همین دلیل برای ایجاد آرایه معمولی معمولاً از:

```javascript
[]
```

استفاده می‌کنیم.

---

# 4. `Array.isArray()`

گاهی لازم است بفهمیم یک مقدار واقعاً Array است یا نه.

برای این کار:

```javascript
Array.isArray(value)
```

مثال:

```javascript
const numbers = [1, 2, 3];

console.log(Array.isArray(numbers));
```

خروجی:

```text
true
```

اما:

```javascript
const person = {
    name: "Sina"
};

console.log(Array.isArray(person));
```

خروجی:

```text
false
```

---

## چرا `Array.isArray()` مهم است؟

چون Array در JavaScript در واقع نوعی Object محسوب می‌شود.

مثلاً:

```javascript
typeof [1, 2, 3]
```

خروجی:

```text
"object"
```

پس اگر بخواهیم بفهمیم یک مقدار Array است:

```javascript
typeof value === "object"
```

کافی نیست.

روش درست:

```javascript
Array.isArray(value)
```

مثلاً:

```javascript
Array.isArray([]);
```

```text
true
```

---

# 5. `push()`

متد `push()` یک یا چند مقدار را **به انتهای Array اضافه می‌کند**.

```javascript
const numbers = [1, 2, 3];

numbers.push(4);

console.log(numbers);
```

خروجی:

```javascript
[1, 2, 3, 4]
```

می‌توانیم چند مقدار همزمان اضافه کنیم:

```javascript
numbers.push(5, 6, 7);
```

نتیجه:

```javascript
[1, 2, 3, 4, 5, 6, 7]
```

---

## `push()` چه چیزی برمی‌گرداند؟

`push()` مقدار **length جدید Array** را برمی‌گرداند.

```javascript
const numbers = [1, 2, 3];

const result = numbers.push(4);

console.log(result);
```

خروجی:

```text
4
```

چون طول جدید Array برابر 4 شده است.

---

# 6. `pop()`

`pop()` آخرین عنصر Array را حذف می‌کند.

```javascript
const numbers = [1, 2, 3];

numbers.pop();

console.log(numbers);
```

خروجی:

```javascript
[1, 2]
```

اما نکته مهم:

`pop()` خود عنصر حذف‌شده را برمی‌گرداند.

```javascript
const numbers = [1, 2, 3];

const removed = numbers.pop();

console.log(removed);
```

خروجی:

```text
3
```

پس:

```text
push() → اضافه کردن به انتها
pop()  → حذف کردن از انتها
```

---

# 7. `unshift()`

`unshift()` یک یا چند عنصر را به **ابتدای Array** اضافه می‌کند.

```javascript
const numbers = [2, 3, 4];

numbers.unshift(1);

console.log(numbers);
```

خروجی:

```javascript
[1, 2, 3, 4]
```

چند مقدار:

```javascript
numbers.unshift(-1, 0);
```

نتیجه:

```javascript
[-1, 0, 1, 2, 3, 4]
```

مثل `push()`، مقدار بازگشتی `unshift()` طول جدید Array است.

---

# 8. `shift()`

`shift()` اولین عنصر Array را حذف می‌کند.

```javascript
const numbers = [1, 2, 3];

const removed = numbers.shift();

console.log(removed);
```

خروجی:

```text
1
```

Array:

```javascript
[2, 3]
```

بنابراین چهار متد مهم را می‌توانیم این‌طور حفظ کنیم:

| متد         | عملیات     | محل   |
| ----------- | ---------- | ----- |
| `push()`    | اضافه کردن | انتها |
| `pop()`     | حذف کردن   | انتها |
| `unshift()` | اضافه کردن | ابتدا |
| `shift()`   | حذف کردن   | ابتدا |

---

# 9. Mutable و Immutable

این مفهوم بسیار مهم است.

## Mutable

**Mutable** یعنی:

> چیزی که بعد از ساخته شدن می‌تواند تغییر کند.

Array در JavaScript یک Object و به طور معمول **mutable** است.

مثلاً:

```javascript
const numbers = [1, 2, 3];

numbers.push(4);

console.log(numbers);
```

نتیجه:

```javascript
[1, 2, 3, 4]
```

خود Array تغییر کرده است.

---

# 10. `const` جلوی تغییر Array را نمی‌گیرد

این نکته با جزوه `let / const` ارتباط مستقیم دارد.

وقتی می‌نویسیم:

```javascript
const numbers = [1, 2, 3];
```

`const` به این معنی نیست که Array immutable شده است.

بلکه یعنی متغیر `numbers` نمی‌تواند به Array دیگری **reassign** شود.

این مجاز است:

```javascript
numbers.push(4);
```

اما این مجاز نیست:

```javascript
numbers = [10, 20, 30];
```

چون `numbers` با `const` تعریف شده است.

---

## تصور ذهنی

```text
numbers
   │
   ▼
[1, 2, 3]
```

`const` اجازه نمی‌دهد:

```text
numbers
   │
   └──────X──────> [10, 20, 30]
```

اما اجازه می‌دهد خود Object/Array را تغییر دهیم:

```text
numbers
   │
   ▼
[1, 2, 3, 4]
```

این همان تفاوت مهم بین:

**reassignment**

و

**mutation**

است.

---

# 11. Immutable

Immutable یعنی:

> مقدار یا ساختار مورد نظر مستقیماً قابل تغییر نیست و برای ایجاد نسخه تغییرکرده باید مقدار جدیدی ساخته شود.

مثلاً Stringها در JavaScript immutable هستند.

```javascript
let name = "Sina";

name.toUpperCase();

console.log(name);
```

هنوز:

```text
Sina
```

داریم.

چون `toUpperCase()` String جدید تولید می‌کند:

```javascript
const result = name.toUpperCase();

console.log(result);
```

```text
SINA
```

Array برخلاف String به طور معمول mutable است.

---

# 12. `reverse()`

`reverse()` ترتیب عناصر Array را برعکس می‌کند.

```javascript
const numbers = [1, 2, 3, 4];

numbers.reverse();

console.log(numbers);
```

خروجی:

```javascript
[4, 3, 2, 1]
```

اما نکته بسیار مهم:

## `reverse()` خود Array را تغییر می‌دهد.

یعنی:

```javascript
const numbers = [1, 2, 3];

const result = numbers.reverse();

console.log(numbers);
console.log(result);
```

هر دو:

```text
[3, 2, 1]
```

را نشان می‌دهند.

---

# 13. ارتباط `reverse()` با Mutable

چون `reverse()` Array را تغییر می‌دهد:

```javascript
const numbers = [1, 2, 3];

numbers.reverse();
```

این یک **mutation** است.

پس اگر نمی‌خواهیم Array اصلی تغییر کند، می‌توانیم ابتدا یک کپی بسازیم:

```javascript
const numbers = [1, 2, 3];

const reversed = [...numbers].reverse();

console.log(numbers);
console.log(reversed);
```

نتیجه:

```text
numbers  → [1, 2, 3]

reversed → [3, 2, 1]
```

اینجا:

```javascript
[...numbers]
```

یک Array جدید می‌سازد.

---

# 14. `flat()`

گاهی یک Array شامل Array دیگری است.

مثلاً:

```javascript
const numbers = [1, 2, [3, 4], 5];

console.log(numbers);
```

ساختار:

```text
[
    1,
    2,
    [
        3,
        4
    ],
    5
]
```

با:

```javascript
numbers.flat();
```

آرایه داخلی یک سطح باز می‌شود:

```javascript
[1, 2, 3, 4, 5]
```

---

## `flat()` چند سطح را باز می‌کند؟

به صورت پیش‌فرض:

```javascript
flat()
```

فقط **یک level** را باز می‌کند.

مثلاً:

```javascript
const numbers = [1, [2, [3, 4]]];

console.log(numbers.flat());
```

نتیجه:

```javascript
[1, 2, [3, 4]]
```

چون فقط یک سطح باز شده است.

---

## باز کردن دو سطح

```javascript
numbers.flat(2);
```

نتیجه:

```javascript
[1, 2, 3, 4]
```

---

## باز کردن تمام سطوح

```javascript
numbers.flat(Infinity);
```

مثلاً:

```javascript
const numbers = [1, [2, [3, [4, 5]]]];

console.log(numbers.flat(Infinity));
```

نتیجه:

```javascript
[1, 2, 3, 4, 5]
```

---

# 15. `concat()`

`concat()` برای اتصال Arrayها یا مقادیر استفاده می‌شود.

```javascript
const a = [1, 2];
const b = [3, 4];

const result = a.concat(b);

console.log(result);
```

خروجی:

```javascript
[1, 2, 3, 4]
```

نکته مهم:

`concat()` Array اصلی را تغییر نمی‌دهد.

```javascript
console.log(a);
```

هنوز:

```javascript
[1, 2]
```

است.

---

## چند Array

```javascript
const a = [1, 2];
const b = [3, 4];
const c = [5, 6];

const result = a.concat(b, c);
```

نتیجه:

```javascript
[1, 2, 3, 4, 5, 6]
```

---

# 16. Object

حالا از Array به Object می‌رسیم.

Object برای نگهداری داده به صورت:

```text
key → value
```

است.

مثلاً:

```javascript
const person = {
    name: "Sina",
    age: 24,
    job: "Developer"
};
```

ساختار:

```text
name → "Sina"
age  → 24
job  → "Developer"
```

---

# 17. Object Assignment

یکی از روش‌های مهم برای کار با Objectها:

```javascript
Object.assign()
```

است.

مثلاً:

```javascript
const person = {
    name: "Sina"
};

const job = {
    title: "Developer"
};

const result = Object.assign(person, job);

console.log(result);
```

نتیجه:

```javascript
{
    name: "Sina",
    title: "Developer"
}
```

---

# 18. نکته مهم درباره `Object.assign()`

فرم کلی:

```javascript
Object.assign(target, source)
```

است.

یعنی:

```text
target ← مقصد
source ← منبع
```

مثلاً:

```javascript
Object.assign(person, car);
```

یعنی properties مربوط به `car` داخل `person` کپی می‌شوند.

---

## Mutation در `Object.assign`

این قسمت بسیار مهم است.

```javascript
const person = {
    name: "Sina"
};

const car = {
    model: "BMW"
};

Object.assign(person, car);
```

خود `person` تغییر کرده است:

```javascript
console.log(person);
```

```javascript
{
    name: "Sina",
    model: "BMW"
}
```

بنابراین اگر بخواهیم Object اصلی تغییر نکند، می‌توانیم Target جدید بسازیم:

```javascript
const result = Object.assign({}, person, car);
```

حالا:

```text
person → تغییر نکرده
car    → تغییر نکرده
result → Object جدید
```

---

# 19. Object Spread Syntax

در JavaScript مدرن معمولاً برای ترکیب Objectها از:

```javascript
...
```

استفاده می‌کنیم.

مثلاً:

```javascript
const person = {
    name: "Sina",
    age: 24
};

const car = {
    model: "BMW",
    year: 2024
};

const concatObject = {
    ...person,
    ...car
};
```

نتیجه:

```javascript
{
    name: "Sina",
    age: 24,
    model: "BMW",
    year: 2024
}
```

---

# 20. Spread یعنی چه؟

در:

```javascript
{
    ...person
}
```

می‌توانیم به صورت ذهنی بگوییم:

> propertyهای داخل `person` را اینجا پخش/کپی کن.

مثلاً:

```javascript
const person = {
    name: "Sina",
    age: 24
};

const copy = {
    ...person
};
```

تقریباً مثل این است:

```javascript
const copy = {
    name: "Sina",
    age: 24
};
```

البته این یک **shallow copy** است؛ یعنی اگر Objectهای تو در تو داشته باشیم، داستان عمیق‌تری دارد.

---

# 21. ترکیب چند Object

می‌توانیم چند Object را ترکیب کنیم:

```javascript
const person = {
    name: "Sina"
};

const car = {
    model: "BMW"
};

const job = {
    title: "Developer"
};

const data = {
    ...person,
    ...car,
    ...job
};
```

نتیجه:

```javascript
{
    name: "Sina",
    model: "BMW",
    title: "Developer"
}
```

---

# 22. اگر property تکراری باشد چه می‌شود؟

مثلاً:

```javascript
const person = {
    name: "Sina"
};

const otherPerson = {
    name: "Ali"
};

const result = {
    ...person,
    ...otherPerson
};
```

نتیجه:

```javascript
{
    name: "Ali"
}
```

چون property بعدی مقدار قبلی را overwrite می‌کند.

یعنی:

```text
person.name       → Sina
otherPerson.name  → Ali

آخرین مقدار       → Ali
```

برعکس:

```javascript
const result = {
    ...otherPerson,
    ...person
};
```

نتیجه:

```javascript
{
    name: "Sina"
}
```

---

# 23. `hasOwnProperty()`

این متد برای بررسی اینکه آیا یک Object **خودش** یک property مشخص دارد یا نه استفاده می‌شود.

مثلاً:

```javascript
const person = {
    name: "Sina",
    age: 24
};

console.log(person.hasOwnProperty("name"));
```

خروجی:

```text
true
```

اما:

```javascript
console.log(person.hasOwnProperty("job"));
```

خروجی:

```text
false
```

---

## چرا `hasOwnProperty()` مفید است؟

مثلاً:

```javascript
const person = {
    name: "Sina",
    age: 24
};

if (person.hasOwnProperty("age")) {
    console.log("Age exists");
}
```

خروجی:

```text
Age exists
```

---

# 24. Own Property یعنی چه؟

در JavaScript بعضی propertyها ممکن است از **prototype** به ارث رسیده باشند.

اما:

```javascript
hasOwnProperty()
```

بررسی می‌کند که property مستقیماً متعلق به خود Object هست یا نه.

این مفهوم با بحث:

```text
Object
Prototype
Prototype Chain
```

ارتباط دارد که بعداً می‌توانیم جداگانه بررسی کنیم.

---

# 25. `Object.keys()`

`Object.keys()` نام تمام **کلیدهای مستقیم Object** را به صورت Array برمی‌گرداند.

مثلاً:

```javascript
const person = {
    name: "Sina",
    age: 24,
    job: "Developer"
};

console.log(Object.keys(person));
```

نتیجه:

```javascript
["name", "age", "job"]
```

پس:

```text
Object
   ↓
keys
   ↓
["name", "age", "job"]
```

---

# 26. `Object.values()`

`Object.values()` تمام **valueها** را به صورت Array برمی‌گرداند.

```javascript
const person = {
    name: "Sina",
    age: 24,
    job: "Developer"
};

console.log(Object.values(person));
```

نتیجه:

```javascript
["Sina", 24, "Developer"]
```

---

# 27. `Object.entries()`

`Object.entries()` هم key و هم value را برمی‌گرداند.

مثلاً:

```javascript
const person = {
    name: "Sina",
    age: 24
};

console.log(Object.entries(person));
```

نتیجه:

```javascript
[
    ["name", "Sina"],
    ["age", 24]
]
```

یعنی هر property تبدیل به یک Array دو عضوی شده است:

```text
["name", "Sina"]
      ↑       ↑
     key    value
```

---

# 28. مقایسه `keys` و `values` و `entries`

فرض کنیم:

```javascript
const person = {
    name: "Sina",
    age: 24
};
```

### `Object.keys()`

```javascript
Object.keys(person);
```

```javascript
["name", "age"]
```

### `Object.values()`

```javascript
Object.values(person);
```

```javascript
["Sina", 24]
```

### `Object.entries()`

```javascript
Object.entries(person);
```

```javascript
[
    ["name", "Sina"],
    ["age", 24]
]
```

پس:

```text
keys
↓
keyها

values
↓
valueها

entries
↓
[key, value]
```

---

# 29. `for...in`

`for...in` برای پیمایش **propertyهای یک Object** بسیار رایج است.

مثلاً:

```javascript
const person = {
    name: "Sina",
    age: 24,
    job: "Developer"
};

for (const key in person) {
    console.log(key);
}
```

خروجی:

```text
name
age
job
```

---

## گرفتن Value

با استفاده از:

```javascript
person[key]
```

می‌توانیم value مربوط به key را بگیریم.

```javascript
for (const key in person) {
    console.log(key, person[key]);
}
```

خروجی:

```text
name Sina
age 24
job Developer
```

---

# 30. چرا `person[key]` و نه `person.key`؟

این نکته بسیار مهم است.

اگر داشته باشیم:

```javascript
const key = "name";
```

این:

```javascript
person[key]
```

یعنی:

```javascript
person["name"]
```

اما:

```javascript
person.key
```

دنبال propertyای به نام:

```text
"key"
```

می‌گردد.

پس:

```javascript
person.key
```

و:

```javascript
person[key]
```

یکسان نیستند.

---

# 31. `for...of`

`for...of` برای پیمایش **iterableها** استفاده می‌شود؛ یکی از مهم‌ترین نمونه‌ها Array است.

مثلاً:

```javascript
const numbers = [10, 20, 30];

for (const number of numbers) {
    console.log(number);
}
```

خروجی:

```text
10
20
30
```

---

# 32. تفاوت `for...in` و `for...of`

این دو را خیلی خوب یاد بگیر:

```text
for...in
↓
property / key

for...of
↓
value
```

مثلاً:

```javascript
const numbers = [10, 20, 30];

for (const index in numbers) {
    console.log(index);
}
```

خروجی:

```text
0
1
2
```

ولی:

```javascript
for (const value of numbers) {
    console.log(value);
}
```

خروجی:

```text
10
20
30
```

---

# 33. یک مثال مقایسه‌ای بسیار مهم

```javascript
const numbers = [10, 20, 30];
```

با:

```javascript
for (const x in numbers) {
    console.log(x);
}
```

داریم:

```text
0
1
2
```

اما:

```javascript
for (const x of numbers) {
    console.log(x);
}
```

داریم:

```text
10
20
30
```

بنابراین:

```text
IN  → index / key
OF  → value
```

البته `for...in` برای Array معمولاً انتخاب مناسبی برای پیمایش مقدارها نیست؛ برای Array معمولاً `for...of` یا متدهای Array مناسب‌ترند.

---

# 34. `Object.entries()` + `for...of`

یکی از ترکیب‌های بسیار کاربردی:

```javascript
const person = {
    name: "Sina",
    age: 24,
    job: "Developer"
};

for (const [key, value] of Object.entries(person)) {
    console.log(key, value);
}
```

نتیجه:

```text
name Sina
age 24
job Developer
```

اینجا:

```javascript
Object.entries(person)
```

تبدیل می‌کند به:

```javascript
[
    ["name", "Sina"],
    ["age", 24],
    ["job", "Developer"]
]
```

و:

```javascript
for (const [key, value] of ...)
```

هر جفت را جدا می‌کند.

این:

```javascript
[key, value]
```

یک نمونه از **destructuring** است.

---

# 35. `Object.freeze()`

`Object.freeze()` برای **فریز کردن Object** استفاده می‌شود.

مثلاً:

```javascript
const person = {
    name: "Sina",
    age: 24
};

Object.freeze(person);
```

بعد:

```javascript
person.age = 30;
```

نمی‌تواند مقدار را تغییر دهد.

همچنین:

```javascript
person.job = "Developer";
```

نمی‌تواند property جدید اضافه کند.

و:

```javascript
delete person.age;
```

نمی‌تواند property را حذف کند.

---

# 36. `freeze()` یعنی Immutable؟

تقریباً، اما یک نکته بسیار مهم وجود دارد.

`Object.freeze()` فقط **shallow** است.

مثلاً:

```javascript
const person = {
    name: "Sina",
    address: {
        city: "Tehran"
    }
};

Object.freeze(person);
```

حالا:

```javascript
person.name = "Ali";
```

تغییر نمی‌کند.

اما:

```javascript
person.address.city = "Shiraz";
```

ممکن است تغییر کند.

چرا؟

چون:

```text
person
  │
  ├── name
  │
  └── address ─────> Object دیگر
                         │
                         └── city
```

`freeze()` خود `person` را فریز کرده، اما Object داخلی `address` را فریز نکرده است.

برای فریز کردن عمیق باید تمام Objectهای داخلی نیز freeze شوند؛ چیزی که به آن **deep freeze** می‌گوییم.

---

# 37. `Object.freeze()` و `const`

این دو را با هم اشتباه نکن.

```javascript
const person = {
    name: "Sina"
};
```

`const` می‌گوید:

> متغیر `person` نمی‌تواند به Object دیگری reassign شود.

اما:

```javascript
person.name = "Ali";
```

مجاز است.

---

اما:

```javascript
Object.freeze(person);
```

می‌گوید:

> تغییر ساختار و propertyهای این Object مجاز نیست.

پس:

```text
const
↓
جلوگیری از reassignment متغیر

Object.freeze()
↓
جلوگیری از mutation سطح Object
```

---

# 38. یک مثال کامل برای درک تفاوت

```javascript
const person = {
    name: "Sina",
    age: 24
};

person.age = 25;
```

مجاز است.

اما:

```javascript
person = {
    name: "Ali"
};
```

خطا می‌دهد.

حالا:

```javascript
const person = {
    name: "Sina",
    age: 24
};

Object.freeze(person);

person.age = 25;
```

تغییر انجام نمی‌شود.

و:

```javascript
person.job = "Developer";
```

نیز مجاز نیست.

---

# 39. `Object.freeze()` در Strict Mode

در حالت عادی، بعضی تلاش‌های غیرمجاز برای تغییر Object فریز شده ممکن است بدون خطای واضح نادیده گرفته شوند.

اما در **Strict Mode** معمولاً JavaScript خطا می‌دهد.

مثلاً:

```javascript
"use strict";

const person = {
    name: "Sina"
};

Object.freeze(person);

person.name = "Ali";
```

می‌تواند باعث `TypeError` شود.

---

# 40. یک نکته مهم: Array هم Object است

این موضوع باعث می‌شود `Object.freeze()` را روی Array هم بتوانیم استفاده کنیم.

```javascript
const numbers = [1, 2, 3];

Object.freeze(numbers);
```

بعد:

```javascript
numbers.push(4);
```

نمی‌تواند Array را تغییر دهد.

همچنین:

```javascript
numbers[0] = 100;
```

تغییر نمی‌کند.

پس:

```text
Array
  ↓
نوع خاصی از Object
```

و به همین دلیل بسیاری از قابلیت‌های Object روی آن قابل استفاده هستند.

---

# 41. ارتباط مفاهیم این جزوه

حالا اگر همه مطالب را کنار هم قرار دهیم:

```text
JavaScript Data
│
├── Primitive
│   ├── String
│   ├── Number
│   ├── Boolean
│   ├── Symbol
│   ├── BigInt
│   ├── Undefined
│   └── Null
│
└── Object
    │
    ├── Object
    │
    └── Array
```

و Array:

```text
Array
│
├── push()
├── pop()
├── shift()
├── unshift()
├── reverse()
├── flat()
└── concat()
```

Object:

```text
Object
│
├── Object.assign()
├── Object.keys()
├── Object.values()
├── Object.entries()
├── Object.freeze()
└── hasOwnProperty()
```

و برای پیمایش:

```text
for...in
↓
key / property

for...of
↓
value
```

---

# 42. Mutable در برابر Immutable

این بخش را به عنوان یکی از نکات کلیدی جزوه حفظ کن.

### Array

به طور معمول:

```javascript
const numbers = [1, 2, 3];

numbers.push(4);
```

خود Array تغییر می‌کند.

پس:

```text
Array → Mutable
```

### Object

به طور معمول:

```javascript
const person = {
    name: "Sina"
};

person.name = "Ali";
```

خود Object تغییر می‌کند.

پس:

```text
Object → Mutable
```

### String

```javascript
const name = "Sina";

name.toUpperCase();
```

خود String تغییر نمی‌کند.

پس:

```text
String → Immutable
```

---

# 43. جدول متدهای مهم Array

| Method      | کار                        |
| ----------- | -------------------------- |
| `push()`    | اضافه کردن به انتها        |
| `pop()`     | حذف از انتها               |
| `unshift()` | اضافه کردن به ابتدا        |
| `shift()`   | حذف از ابتدا               |
| `reverse()` | معکوس کردن Array           |
| `flat()`    | باز کردن Arrayهای تو در تو |
| `concat()`  | اتصال Arrayها              |

### از نظر Mutation

| Method      | Array اصلی را تغییر می‌دهد؟ |
| ----------- | --------------------------- |
| `push()`    | ✅                           |
| `pop()`     | ✅                           |
| `unshift()` | ✅                           |
| `shift()`   | ✅                           |
| `reverse()` | ✅                           |
| `flat()`    | ❌                           |
| `concat()`  | ❌                           |

این جدول خیلی مهم است.

---

# 44. جدول متدهای مهم Object

| Method             | کاربرد                      |
| ------------------ | --------------------------- |
| `Object.assign()`  | کپی/ترکیب propertyها        |
| `Object.keys()`    | دریافت keyها                |
| `Object.values()`  | دریافت valueها              |
| `Object.entries()` | دریافت `[key, value]`       |
| `Object.freeze()`  | جلوگیری از mutation سطح اول |
| `hasOwnProperty()` | بررسی وجود property مستقیم  |

---

# 45. `for...in` در برابر `for...of`

|                        | `for...in`        | `for...of`                 |
| ---------------------- | ----------------- | -------------------------- |
| مناسب برای             | Object properties | Iterable values            |
| خروجی در Array         | index             | value                      |
| خروجی در Object معمولی | key               | مستقیماً قابل استفاده نیست |
| مثال                   | `0, 1, 2`         | `10, 20, 30`               |

مثلاً:

```javascript
const numbers = [10, 20, 30];

for (const index in numbers) {
    console.log(index);
}
```

```text
0
1
2
```

در مقابل:

```javascript
for (const value of numbers) {
    console.log(value);
}
```

```text
10
20
30
```

---

# 46. یک مثال ترکیبی

حالا چند مفهوم را با هم استفاده کنیم:

```javascript
const person = {
    name: "Sina",
    age: 24
};

const car = {
    brand: "BMW",
    model: "M3"
};

const data = {
    ...person,
    ...car
};

console.log(data);
```

نتیجه:

```javascript
{
    name: "Sina",
    age: 24,
    brand: "BMW",
    model: "M3"
}
```

حالا:

```javascript
console.log(Object.keys(data));
```

```javascript
["name", "age", "brand", "model"]
```

و:

```javascript
console.log(Object.values(data));
```

```javascript
["Sina", 24, "BMW", "M3"]
```

و:

```javascript
console.log(Object.entries(data));
```

```javascript
[
    ["name", "Sina"],
    ["age", 24],
    ["brand", "BMW"],
    ["model", "M3"]
]
```

و:

```javascript
for (const [key, value] of Object.entries(data)) {
    console.log(`${key}: ${value}`);
}
```

خروجی:

```text
name: Sina
age: 24
brand: BMW
model: M3
```

---

# 47. مثال ترکیبی Array

```javascript
const numbers = [1, 2, 3];

numbers.push(4);

numbers.unshift(0);

console.log(numbers);
```

نتیجه:

```javascript
[0, 1, 2, 3, 4]
```

حالا:

```javascript
numbers.pop();
```

نتیجه:

```javascript
[0, 1, 2, 3]
```

و:

```javascript
numbers.shift();
```

نتیجه:

```javascript
[1, 2, 3]
```

سپس:

```javascript
numbers.reverse();
```

نتیجه:

```javascript
[3, 2, 1]
```

---

# 48. یک نکته بسیار مهم درباره `Array` و `Object`

در JavaScript وقتی می‌نویسیم:

```javascript
const a = [1, 2, 3];
const b = a;
```

یک Array جدید ساخته نشده است.

بلکه:

```text
a ──────┐
        ↓
    [1, 2, 3]
        ↑
        │
b ──────┘
```

بنابراین:

```javascript
b.push(4);

console.log(a);
```

نتیجه:

```javascript
[1, 2, 3, 4]
```

چون `a` و `b` به همان Object اشاره می‌کنند.

این همان مفهوم **Reference** است که در بحث Memory و Heap به آن رسیدیم.

---

# 49. Spread و Reference

حالا:

```javascript
const a = [1, 2, 3];

const b = [...a];

b.push(4);

console.log(a);
console.log(b);
```

نتیجه:

```text
a → [1, 2, 3]

b → [1, 2, 3, 4]
```

چون:

```javascript
[...a]
```

یک Array جدید ساخته است.

برای Object هم:

```javascript
const person = {
    name: "Sina"
};

const copy = {
    ...person
};
```

یک Object جدید ایجاد می‌کند.

---

# 50. Shallow Copy

اما یک نکته بسیار مهم:

```javascript
const person = {
    name: "Sina",
    address: {
        city: "Tehran"
    }
};

const copy = {
    ...person
};
```

اینجا `copy` و `person` دو Object جدا هستند، اما:

```javascript
person.address
```

و:

```javascript
copy.address
```

به همان Object داخلی اشاره می‌کنند.

پس:

```javascript
copy.address.city = "Shiraz";
```

ممکن است:

```javascript
console.log(person.address.city);
```

را هم تغییر دهد.

این یعنی Spread و `Object.assign()` **shallow copy** انجام می‌دهند، نه deep copy.

---

# 51. نقشه ذهنی نهایی

```text
                         JavaScript
                              │
                    ┌─────────┴─────────┐
                    │                   │
               Primitive             Object
                                        │
                               ┌────────┴────────┐
                               │                 │
                            Object             Array
                               │                 │
                 ┌─────────────┼───────┐         │
                 │             │       │         │
              keys()       values() entries()   push()
                                                 pop()
                                                 shift()
                                                 unshift()
                                                 reverse()
                                                 flat()
                                                 concat()
```

برای Objectها:

```text
Object
│
├── Object.assign()
├── Object.keys()
├── Object.values()
├── Object.entries()
├── Object.freeze()
└── hasOwnProperty()
```

برای پیمایش:

```text
for...in
    ↓
key / index

for...of
    ↓
value
```

و از نظر تغییرپذیری:

```text
Mutable
│
├── Object
└── Array

Immutable
│
└── String
```

با یک استثنای مهم:

```javascript
Object.freeze()
```

می‌تواند mutation سطح اول یک Object یا Array را مسدود کند.

---

## خلاصه خیلی فشرده برای مرور

```javascript
const arr = [1, 2, 3];

arr.push(4);        // اضافه به انتها
arr.pop();          // حذف از انتها
arr.unshift(0);     // اضافه به ابتدا
arr.shift();        // حذف از ابتدا

arr.reverse();      // معکوس، MUTATE می‌کند
arr.flat();         // باز کردن یک level
arr.concat([4, 5]); // ترکیب، Array جدید
```

```javascript
Array.isArray(arr);
```

برای تشخیص Array.

---

```javascript
const person = {
    name: "Sina",
    age: 24
};

Object.keys(person);
Object.values(person);
Object.entries(person);

person.hasOwnProperty("name");

Object.freeze(person);
```

---

```javascript
const concatObject = {
    ...person,
    ...car
};
```

برای ساخت Object جدید از چند Object.

---

```javascript
for (const key in person) {
    console.log(key);
}
```

یعنی:

```text
key
```

و:

```javascript
for (const value of arr) {
    console.log(value);
}
```

یعنی:

```text
value
```

---

### سه نکته‌ای که واقعاً ارزش دارد همین الان در ذهنت تثبیت شوند

1. **`const` به معنی immutable بودن Array/Object نیست.**

```javascript
const arr = [1, 2];

arr.push(3); // ✅
arr = [4, 5]; // ❌
```

2. **Array و Object معمولاً mutable هستند و با Reference درگیرند.**

```javascript
const a = [1, 2];
const b = a;

b.push(3);

// a هم تغییر کرده
```

3. **`for...in` و `for...of` را قاطی نکن:**

```text
in → key / index
of → value
```

این سه مفهوم، مخصوصاً در کنار مباحث قبلی `const`، `Heap/Reference` و `typeof`، پایه‌ی بخش بزرگی از JavaScript هستند.
