# JavaScript Technical Paper

---

## Different Datatypes in JavaScript

JavaScript can store different kinds of data in variables.

**Types:**
- **Number:** Any number like 5, 3.14, -10
- **String:** Text in quotes like "hello"
- **Boolean:** true or false
- **Null:** Empty on purpose
- **Undefined:** Not given a value yet
- **Object:** Can store multiple values like {name: "John"}
- **Array:** List of items like [1, 2, 3]

**Example:**

```javascript
let age = 20;                    // number
let name = "Sam";                // string
let isStudent = true;            // boolean
let nothing = null;              // null
let notSet;                      // undefined
let person = {name: "John"};     // object
let colors = ["red", "blue"];    // array
```

---

## Scopes in JavaScript

Scope means where you can use a variable.

- **Global Scope:** Variable can be used anywhere
- **Function Scope:** Variable only works inside a function
- **Block Scope:** Variable only works inside { }

**Example:**

```javascript
let x = 10;  // global - works everywhere

function test() {
    let y = 20;  // function scope - only in this function
    
    if (true) {
        let z = 30;  // block scope - only in this { }
    }
}
```

---

## let, var, const

| Feature | var | let | const |
| ------- | --- | --- | ----- |
| Scope | Function | Block | Block |
| Can redeclare? | Yes | No | No |
| Can change value? | Yes | Yes | No |

**Example:**

```javascript
var a = 1;
let b = 2;
const c = 3;

a = 10;  // works
b = 20;  // works
c = 30;  // ERROR! const cannot change
```

---

## Why We Must Not Use var

`var` has problems - it leaks out of blocks and can be confusing.

**Example:**

```javascript
if (true) {
    var x = 5;
}
console.log(x);  // 5 - this is bad! x leaked out
```

**Better way:**

```javascript
if (true) {
    let x = 5;
}
console.log(x);  // ERROR - this is good! x stays inside
```

---

## Why Using Global Variables Is Bad

Global variables can cause problems:
- Hard to find bugs
- Other code can accidentally change them
- Makes code messy
- Hard to reuse code

**Bad example:**

```javascript
let score = 0;  // global - anyone can change this

function game1() {
    score = 100;
}

function game2() {
    score = 0;  // Oops! Changed game1's score
}
```

---

## Truthy and Falsy Values

JavaScript treats some values as true or false in conditions.

**Falsy (treated as false):**
- 0
- ""
- null
- undefined
- false
- NaN

**Truthy (treated as true):**
- Everything else
- "hello"
- 1, -1, 100
- [ ]
- { }

**Example:**

```javascript
if (0) {
    console.log("Won't run");  // 0 is falsy
}

if ("hello") {
    console.log("Will run");  // "hello" is truthy
}
```

---

## Function Hoisting

You can call a function before you write it in code.

**Example:**

```javascript
sayHi();  // This works!

function sayHi() {
    console.log("Hi!");
}
```

But this doesn't work:

```javascript
greet();  // ERROR!

const greet = function() {
    console.log("Hello");
}
```

---

## What Happens When a Function Does Not Have a Return Statement

If a function doesn't return anything, it returns `undefined`.

**Example:**

```javascript
function add(a, b) {
    let sum = a + b;
    // no return
}

let result = add(2, 3);
console.log(result);  // undefined
```

---

## Different Ways of Declaring a Function

**Way 1: Regular function**
```javascript
function greet() {
    console.log("Hello");
}
```

**Way 2: Function in a variable**
```javascript
const greet = function() {
    console.log("Hello");
}
```

**Way 3: Arrow function**
```javascript
const greet = () => {
    console.log("Hello");
}
```

---

## Pass by Reference and Pass by Value

**Pass by Value (Numbers, Strings, Booleans):**
Makes a copy. Changing the copy doesn't change the original.

```javascript
let a = 10;
let b = a;      // b gets a COPY of 10
b = 20;

console.log(a); // 10 (not changed)
console.log(b); // 20
```

**Pass by Reference (Objects, Arrays):**
Shares the same data. Changing one changes both.

```javascript
let person1 = {name: "John"};
let person2 = person1;  // person2 points to SAME object
person2.name = "Mike";

console.log(person1.name); // "Mike" (changed!)
console.log(person2.name); // "Mike"
```

**To copy an object properly:**
```javascript
let person1 = {name: "John"};
let person2 = {...person1};  // Makes a real copy
person2.name = "Mike";

console.log(person1.name); // "John" (not changed)
```

---

## Different Types of For Loops

**For with numbers:**
```javascript
for (let i = 0; i < 5; i++) {
    console.log(i);  // 0, 1, 2, 3, 4
}
```

**For...in (for object keys):**
```javascript
let person = {name: "Sam", age: 25};
for (let key in person) {
    console.log(key);  // "name", "age"
}
```

**For...of (for array values):**
```javascript
let colors = ["red", "green", "blue"];
for (let color of colors) {
    console.log(color);  // "red", "green", "blue"
}
```

**forEach:**
```javascript
let numbers = [1, 2, 3];
numbers.forEach(num => {
    console.log(num);  // 1, 2, 3
});
```

**While:**
```javascript
let i = 0;
while (i < 3) {
    console.log(i);  // 0, 1, 2
    i++;
}
```

---

## Searching MDN (Mozilla Developer Network)

MDN is the best place to learn JavaScript.

Website: https://developer.mozilla.org

Use it to search for any JavaScript method or concept.

---

## Popular Array Utility Methods

### Basics

**Array.pop (Mutable - changes the array)**
```javascript
let fruits = ["apple", "banana", "orange"];
fruits.pop();  // Removes "orange"
console.log(fruits);  // ["apple", "banana"]
```

**Array.push (Mutable - changes the array)**
```javascript
let fruits = ["apple", "banana"];
fruits.push("orange");  // Adds "orange"
console.log(fruits);  // ["apple", "banana", "orange"]
```

**Array.concat (Immutable - doesn't change original)**
```javascript
let arr1 = [1, 2];
let arr2 = [3, 4];
let combined = arr1.concat(arr2);  // [1, 2, 3, 4]
console.log(arr1);  // [1, 2] (not changed)
```

**Array.slice (Immutable - doesn't change original)**
```javascript
let numbers = [1, 2, 3, 4, 5];
let part = numbers.slice(1, 3);  // [2, 3]
console.log(numbers);  // [1, 2, 3, 4, 5] (not changed)
```

**Array.splice (Mutable - changes the array)**
```javascript
let colors = ["red", "green", "blue", "yellow"];
colors.splice(1, 2);  // Removes 2 items starting at index 1
console.log(colors);  // ["red", "yellow"]
```

**Array.join (Immutable - doesn't change original)**
```javascript
let words = ["Hello", "World"];
let sentence = words.join(" ");  // "Hello World"
console.log(words);  // ["Hello", "World"] (not changed)
```

**Array.flat (Immutable - doesn't change original)**
```javascript
let nested = [1, [2, 3], [4, [5]]];
let flat = nested.flat(2);  // [1, 2, 3, 4, 5]
console.log(nested);  // [1, [2, 3], [4, [5]]] (not changed)
```

### Finding

**Array.find (Immutable)**
```javascript
let numbers = [5, 12, 8, 20];
let big = numbers.find(num => num > 10);  // 12
```

**Array.indexOf (Immutable)**
```javascript
let fruits = ["apple", "banana", "orange"];
let position = fruits.indexOf("banana");  // 1
```

**Array.includes (Immutable)**
```javascript
let numbers = [1, 2, 3];
let hasTwo = numbers.includes(2);  // true
```

**Array.findIndex (Immutable)**
```javascript
let ages = [10, 15, 20, 25];
let index = ages.findIndex(age => age > 18);  // 2
```

### Higher Order Functions

**Array.forEach (Immutable - but used for side effects)**
```javascript
let numbers = [1, 2, 3];
numbers.forEach(num => {
    console.log(num * 2);  // 2, 4, 6
});
```

**Array.filter (Immutable)**
```javascript
let numbers = [1, 2, 3, 4, 5];
let big = numbers.filter(num => num > 3);  // [4, 5]
```

**Array.map (Immutable)**
```javascript
let numbers = [1, 2, 3];
let doubled = numbers.map(num => num * 2);  // [2, 4, 6]
```

**Array.reduce (Immutable)**
```javascript
let numbers = [1, 2, 3, 4];
let sum = numbers.reduce((total, num) => total + num, 0);  // 10
```

**Array.sort (Mutable - changes the array)**
```javascript
let numbers = [3, 1, 4, 2];
numbers.sort((a, b) => a - b);  // [1, 2, 3, 4]
```

### Advanced

**Array Methods Chaining (Immutable)**
```javascript
let numbers = [1, 2, 3, 4, 5];
let result = numbers
    .filter(n => n > 2)      // [3, 4, 5]
    .map(n => n * 2)         // [6, 8, 10]
    .reduce((sum, n) => sum + n, 0);  // 24
```

---

## Popular String Utility Methods

All string methods are **immutable** - they don't change the original string.

**String.trim**
```javascript
let text = "  hello  ";
let clean = text.trim();  // "hello"
console.log(text);  // "  hello  " (not changed)
```

**String.split**
```javascript
let text = "apple,banana,orange";
let fruits = text.split(",");  // ["apple", "banana", "orange"]
```

**String.toUpperCase**
```javascript
let name = "john";
let upper = name.toUpperCase();  // "JOHN"
console.log(name);  // "john" (not changed)
```

**String.replace**
```javascript
let text = "I like cats";
let newText = text.replace("cats", "dogs");  // "I like dogs"
console.log(text);  // "I like cats" (not changed)
```

---

## Popular Object Utility Methods

All object methods are **immutable** - they don't change the original object.

**Object.keys**
```javascript
let person = {name: "John", age: 30};
let keys = Object.keys(person);  // ["name", "age"]
```

**Object.values**
```javascript
let person = {name: "John", age: 30};
let values = Object.values(person);  // ["John", 30]
```

**Object.entries**
```javascript
let person = {name: "John", age: 30};
let entries = Object.entries(person);  // [["name", "John"], ["age", 30]]
```

**Object.assign**
```javascript
let person = {name: "John"};
let copy = Object.assign({}, person);  // Makes a copy
copy.name = "Mike";
console.log(person.name);  // "John" (not changed)
```

---

## When to Use forEach, When to Use Array Utility Methods Like map, filter, reduce

| Method | When to Use |
| ------ | ----------- |
| forEach | When you want to do something with each item (print, save to database) |
| map | When you want to change each item and get a new array |
| filter | When you want to keep only some items |
| reduce | When you want to combine all items into one value (like sum) |

**Example:**

```javascript
let numbers = [1, 2, 3, 4, 5];

// forEach - just do something
numbers.forEach(n => console.log(n));

// map - transform each item
let doubled = numbers.map(n => n * 2);  // [2, 4, 6, 8, 10]

// filter - keep some items
let big = numbers.filter(n => n > 3);  // [4, 5]

// reduce - combine into one value
let sum = numbers.reduce((total, n) => total + n, 0);  // 15
```

---

## Immutable and Mutable Methods

**Mutable:** Changes the original
- Examples: push, pop, splice, sort

**Immutable:** Doesn't change the original, makes a new one
- Examples: map, filter, concat, slice

**Example:**

```javascript
// Mutable
let arr1 = [1, 2, 3];
arr1.push(4);
console.log(arr1);  // [1, 2, 3, 4] (changed!)

// Immutable
let arr2 = [1, 2, 3];
let arr3 = arr2.concat(4);
console.log(arr2);  // [1, 2, 3] (not changed)
console.log(arr3);  // [1, 2, 3, 4] (new array)
```

---

## Error Handling (Try Catch)

Try-catch helps your program not crash when something goes wrong.

```javascript
try {
    // Code that might have an error
    let result = 10 / 0;
    console.log(result);
}
catch (error) {
    console.log("Something went wrong!");
}
```

---

## Throwing Errors

You can make your own errors.

```javascript
function divide(a, b) {
    if (b === 0) {
        throw new Error("Can't divide by zero!");
    }
    return a / b;
}

try {
    divide(10, 0);
}
catch (error) {
    console.log(error.message);  // "Can't divide by zero!"
}
```

---

## Difference Between throw new Error("Error message here") and throw "Error message here"

| throw "message" | throw new Error("message") |
| --------------- | -------------------------- |
| Just throws text | Throws proper error |
| No extra info | Has stack trace (shows where error happened) |
| Don't use this | Use this! |

**Example:**

```javascript
// Bad
throw "Something broke";

// Good
throw new Error("Something broke");
```

---

## Reading Error Messages and Tracing Issues from the Stack Trace When Errors Happen

**Practice this daily for 2 weeks - very important!**

When you get an error, read it from bottom to top.

**Example error:**

```
Error: User not found
    at getUser (app.js:10)
    at showProfile (app.js:20)
    at main (app.js:30)
```

**How to read it:**
1. Line 30: main() was called
2. Line 20: main() called showProfile()
3. Line 10: showProfile() called getUser()
4. Error happened in getUser()

This tells you the path the code took before crashing.

---

## Importance of Catch Block

Catch block is important because:
- Stops your program from crashing
- Lets you show nice error messages to users
- Helps you log errors to fix them later
- Lets you try a backup plan

**Without catch:**
```javascript
let data = JSON.parse(badData);  // Program crashes here
console.log("This never runs");
```

**With catch:**
```javascript
try {
    let data = JSON.parse(badData);
}
catch (error) {
    console.log("Bad data, using default");
    let data = {};
}
console.log("This runs!");  // Program continues
```

---

## Spread Operator

Spread operator (...) unpacks arrays or objects.

**With arrays:**
```javascript
let arr1 = [1, 2];
let arr2 = [...arr1, 3, 4];  // [1, 2, 3, 4]
```

**With objects:**
```javascript
let person = {name: "John", age: 25};
let updated = {...person, age: 26};  // {name: "John", age: 26}
```

**Copy array:**
```javascript
let original = [1, 2, 3];
let copy = [...original];
```

---

## Template Literals

Template literals use backticks (`) to make strings easier.

**Old way:**
```javascript
let name = "Sam";
let age = 20;
let message = "Hi, I'm " + name + " and I'm " + age + " years old";
```

**New way:**
```javascript
let name = "Sam";
let age = 20;
let message = `Hi, I'm ${name} and I'm ${age} years old`;
```

---

## Default Parameters

Give parameters default values if nothing is passed.

```javascript
function greet(name = "Guest") {
    console.log(`Hello, ${name}!`);
}

greet();         // "Hello, Guest!"
greet("John");   // "Hello, John!"
```

---

## Destructuring

Destructuring pulls values out of arrays or objects.

**With objects:**
```javascript
let person = {name: "John", age: 30};
let {name, age} = person;
console.log(name);  // "John"
console.log(age);   // 30
```

**With arrays:**
```javascript
let colors = ["red", "green", "blue"];
let [first, second] = colors;
console.log(first);   // "red"
console.log(second);  // "green"
```

---

## Closures

A closure is when a function remembers variables from where it was created.

```javascript
function makeCounter() {
    let count = 0;
    
    return function() {
        count++;
        console.log(count);
    }
}

let counter = makeCounter();
counter();  // 1
counter();  // 2
counter();  // 3
```

The inner function remembers `count` even after `makeCounter()` finished.

---

## Difference Between Arrow Functions and Regular Functions

| Arrow Functions | Regular Functions |
| --------------- | ----------------- |
| () => {} | function() {} |
| Shorter to write | Longer |
| `this` comes from outside | `this` comes from inside |

**Example:**

```javascript
// Regular
function add(a, b) {
    return a + b;
}

// Arrow
const add = (a, b) => a + b;
```

---

## Difference Between === and ==

**== (loose):** Converts types then compares
**=== (strict):** Compares type AND value

```javascript
// == converts types
0 == false       // true
"5" == 5         // true
null == undefined // true

// === doesn't convert
0 === false      // false
"5" === 5        // false
null === undefined // false
```

**Always use ===** to avoid surprises!

---

## Why Using value === undefined Is Better Than Using !value

**Problem with !value:**
```javascript
let age = 0;

if (!age) {
    console.log("Age not set");  // Wrong! Age IS set to 0
}
```

**Better way:**
```javascript
let age = 0;

if (age === undefined) {
    console.log("Age not set");  // Correct! Won't run
}
```

`!value` treats 0, "", and false as undefined, which is wrong.

---

## Array Utility Methods Chaining

Chain methods together to do multiple things at once.

```javascript
let numbers = [1, 2, 3, 4, 5, 6];

let result = numbers
    .filter(n => n > 2)           // [3, 4, 5, 6]
    .map(n => n * 2)              // [6, 8, 10, 12]
    .reduce((sum, n) => sum + n); // 36

console.log(result);  // 36
```

---

## Difference Between null and undefined

| null | undefined |
| ---- | --------- |
| You set it to empty on purpose | JavaScript sets it when you forget to assign |
| "No value here" | "Oops, not assigned yet" |

**Example:**

```javascript
let a = null;       // You decided it's empty
let b;              // You forgot to assign, so undefined

console.log(a);  // null
console.log(b);  // undefined
```

---

## Importing and Exporting Modules Using require and module.exports

Split your code into different files.

**File: math.js**
```javascript
function add(a, b) {
    return a + b;
}

module.exports = add;
```

**File: app.js**
```javascript
const add = require('./math');
console.log(add(2, 3));  // 5
```

---

## The Different Methods of console

```javascript
console.log("Normal message");           // White text
console.error("Error message");          // Red text
console.warn("Warning message");         // Yellow text
console.info("Info message");            // Blue text
console.table([{a: 1}, {b: 2}]);        // Shows as table
console.clear();                         // Clears console
```

---

## All the Best Practices Mentioned on the LMS

**Indentation:**
```javascript
// Good
function test() {
    if (true) {
        console.log("Hi");
    }
}

// Bad
function test() {
if (true) {
console.log("Hi");
}
}
```

**Variable naming:**
```javascript
// Good
let userName = "John";
let userAge = 25;

// Bad
let un = "John";
let x = 25;
```

**Loop variable naming:**
```javascript
// For numbers, use i, j, k
for (let i = 0; i < 5; i++) {}

// For items, use descriptive names
users.forEach(user => {
    console.log(user.name);
});
```

**Other tips:**
- Keep functions small
- Don't nest too deep
- Add comments for complex code
- Use const when value won't change

---

## Passing Functions to Other Functions and Invoking Them on Demand

You can pass a function to another function.

```javascript
function doSomething(callback) {
    console.log("Starting...");
    callback();
    console.log("Done!");
}

doSomething(() => {
    console.log("Doing the thing");
});

// Output:
// Starting...
// Doing the thing
// Done!
```

---

## Differences Between Named Functions and Anonymous Functions

**Named function:**
```javascript
function sayHi() {
    console.log("Hi!");
}
```

**Anonymous function:**
```javascript
const sayHi = function() {
    console.log("Hi!");
}

// Arrow function (always anonymous)
const sayHi = () => {
    console.log("Hi!");
}
```

---

## Variable Number of Arguments Passed to Functions

Use `...` to accept any number of arguments.

```javascript
function sum(...numbers) {
    let total = 0;
    for (let num of numbers) {
        total += num;
    }
    return total;
}

console.log(sum(1, 2));           // 3
console.log(sum(1, 2, 3));        // 6
console.log(sum(1, 2, 3, 4, 5));  // 15
```

---

## Debugging Strategies

**1. Console.log everything:**
```javascript
function calculate(a, b) {
    console.log("a is:", a);
    console.log("b is:", b);
    let result = a + b;
    console.log("result is:", result);
    return result;
}
```

**2. Use debugger:**
```javascript
function test() {
    let x = 10;
    debugger;  // Code stops here
    console.log(x);
}
```

**3. Check errors from bottom to top:**
- Read the stack trace
- Find which function crashed
- Check that line number

**4. Test small parts:**
- Don't test everything at once
- Test one function at a time

**5. Use browser DevTools:**
- Set breakpoints
- Watch variables
- Step through code

---

## References

- https://www.youtube.com/playlist?list=PLGjplNEQ1it_oTvuLRNqXfz_v_0pq6unW
- https://developer.mozilla.org/en-US/docs/Web/JavaScript
