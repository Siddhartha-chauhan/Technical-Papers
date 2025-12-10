## AsynchronousJS - Technical Paper

---

## 1. How JS Runs Code

JavaScript works on a single thread and runs synchronously by default. Think of it like a to-do list where you complete one task before moving to the next.

There's something called the Call Stack that keeps track of what's running. When you call a function, it gets added to the stack. Once it's done, it gets removed.

**Quick example:**

```javascript
function sayHello() { console.log("Hello"); }
function sayBye() { console.log("Bye"); }
sayHello();
sayBye();

// Output:
// Hello
// Bye
```

---

## 2. Sync vs Async - What's the Difference?

| Aspect | Synchronous | Asynchronous |
| ------ | ----------- | ------------ |
| How it runs | One line at a time | Doesn't wait, keeps going |
| Does it block? | Yes, waits for completion | No, runs in background |
| Examples | Regular functions, loops | setTimeout, API calls, promises |

**Simple demo:**

```javascript
console.log("First");
setTimeout(() => console.log("Middle but appears last"), 2000);
console.log("Last but appears second");

// Output:
// First
// Last but appears second
// Middle but appears last
```

---

## 3. Ways to Write Async Code

Here are the main tools I use for async operations:

- `setTimeout()` and `setInterval()`
- Browser APIs like `fetch()`, event listeners
- Promises
- `async/await` syntax
- Web Workers for heavy tasks

---

## 4. What Are Browser APIs?

These are built-in browser features that let you do complex stuff without freezing your JavaScript code. They run separately from the JS engine.

**Common ones I use:**

- `fetch()` for network requests
- Event listeners for user interactions
- LocalStorage for saving data
- Geolocation API
- Timer functions

---

## 5. The Event Loop Explained

The Event Loop is like a traffic controller. It constantly checks if the Call Stack is empty, and if it is, it grabs the next task from the queue and runs it.

**Simple flow:**

```
Call Stack ← Event Loop ← Task Queue ← Browser APIs
```

---

## 6. Callback Hell (The Pyramid of Doom)

This happens when you nest callbacks inside callbacks inside more callbacks. The code becomes really hard to read and maintain.

**Example of the mess:**

```javascript
setTimeout(() => {
    console.log("Step 1");
    setTimeout(() => {
        console.log("Step 2");
        setTimeout(() => {
            console.log("Step 3");
        }, 1000);
    }, 1000);
}, 1000);
```

Yeah, it gets ugly fast.

---

## 7. Inversion of Control Issue

When you hand off a callback to another function, you're basically giving up control. You don't know when or how that callback will run.

**Example:**

```javascript
function doSomething(callback) {
    callback(); // You're not in control anymore
}
```

---

# Working with Promises

---

## 8. What's a Promise?

A Promise is basically an object that says "I'll give you a result later - either success or failure." It's how we handle async operations in a cleaner way.

---

## 9. Creating Your Own Promise

**Here's how:**

```javascript
const myPromise = new Promise((resolve, reject) => {
    let success = true;
    if (success) {
        resolve("It worked!");
    } else {
        reject("Something failed");
    }
});
```

---

## 10. Promise States

A promise can be in one of three states:

| State | What it means |
| ----- | ------------- |
| Pending | Still waiting for result |
| Fulfilled | Success! `resolve()` was called |
| Rejected | Failed! `reject()` was called |

---

## 11. Using an Existing Promise

**Basic usage:**

```javascript
myPromise
    .then(result => console.log(result))
    .catch(error => console.log(error));
```

---

# Promise Chaining

---

## 12. Chaining Promises with .then()

You can chain multiple `.then()` calls together. Just make sure to return a value so the next `.then()` gets it.

**Example:**

```javascript
myPromise
    .then(data => {
        console.log(data);
        return "Moving to next step";
    })
    .then(nextData => console.log(nextData));
```

---

## 13. Catching Errors in the Chain

Use `.catch()` to handle any errors that happen in the promise chain.

**Example:**

```javascript
myPromise
    .then(response => JSON.parse(response))
    .catch(error => console.log("Oops:", error));
```

---

## 14. The .finally() Block

This runs no matter what happens - success or failure. Great for cleanup tasks.

**Example:**

```javascript
myPromise
    .finally(() => console.log("This always runs"));
```

---

## 15. Error in .then() with .catch() Present

When an error gets thrown inside a `.then()`, it immediately jumps to the nearest `.catch()` block, skipping any `.then()` blocks in between.

**Example:**

```javascript
myPromise
    .then(() => { 
        throw new Error("Oops!"); 
    })
    .catch(err => console.log(err.message));
```

---

## 16. Error in .then() WITHOUT .catch()

If there's no `.catch()`, you get an unhandled promise rejection. This is bad and will show warnings in the console.

**Example:**

```javascript
myPromise
    .then(() => { 
        throw new Error("No one will catch this!"); 
    });
// Result: Unhandled rejection warning
```

---

## 17. Why Put .catch() at the End?

Because errors bubble down the chain. A `.catch()` at the end will catch errors from any `.then()` above it, making your error handling centralized and clean.

---

## 18. Chaining Multiple Promises

Return a new promise inside `.then()` to chain them properly. The next `.then()` will wait for that promise to resolve.

**Example:**

```javascript
fetchUser()
    .then(user => {
        console.log(user);
        return fetchPosts(user.id);
    })
    .then(posts => {
        console.log(posts);
        return fetchComments(posts[0].id);
    })
    .then(comments => {
        console.log(comments);
    })
    .catch(error => {
        console.error("Something broke:", error);
    });
```

---

## 19. Running Multiple Promises with Promise.all()

`Promise.all()` lets you run multiple promises at the same time and wait for all of them to finish. You get an array of results back.

**Example:**

```javascript
Promise.all([getUser(), getPosts(), getComments()])
    .then(results => {
        console.log(results); // [userData, postsData, commentsData]
    })
    .catch(error => {
        console.error("One of them failed:", error);
    });
```

---

## 20. Handling Errors Properly

Error handling is crucial with promises. Here are different approaches:

### Standard .catch() at the end

```javascript
fetchData()
    .then(result => {
        console.log(result);
    })
    .catch(error => {
        console.error("Error happened:", error);
    });
```

### Throwing errors in the chain

```javascript
processData()
    .then(result => {
        if (!result) throw new Error("Invalid data");
        return transformData(result);
    })
    .then(transformed => console.log(transformed))
    .catch(err => console.log("Caught error:", err));
```

### Catching specific parts

```javascript
stepOne()
    .then(res => stepTwo(res))
    .catch(err => console.log("Error in step 1-2"))
    .then(() => stepThree())
    .catch(err => console.log("Error in step 3"));
```

### Using .finally() for cleanup

```javascript
performTask()
    .then(res => console.log(res))
    .catch(err => console.log(err))
    .finally(() => console.log("Cleanup complete"));
```

### Error handling with Promise.all

```javascript
Promise.all([task1(), task2(), task3()])
    .then(results => console.log(results))
    .catch(err => console.log("At least one failed:", err));
```

Note: If even one promise fails, the whole thing fails.

### Using try-catch with async/await

```javascript
async function executeSteps() {
    try {
        const result1 = await step1();
        const result2 = await step2();
        console.log(result1, result2);
    } catch (err) {
        console.error("Error caught:", err);
    }
}
executeSteps();
```

---

## 21. Why Error Handling is Critical

Error handling isn't optional with promises - it's essential. Here's why:

**Promises are async:** Network calls fail, servers go down, data gets corrupted. Without `.catch()`, you won't know anything went wrong.

**Keeps the chain from breaking:** When an error hits, the rest of your `.then()` chain stops. Having `.catch()` handles it gracefully instead of crashing everything.

**Makes code predictable:** You always know what happens on failure, so you can retry, log the issue, or show user-friendly messages.

**Prevents crashes:** Without error handling, you get scary warnings like:
```
UnhandledPromiseRejectionWarning
```

**Better UX:** Instead of a frozen app, you can:
- Display helpful error messages
- Retry the operation
- Have backup plans ready

**Quick example:**

```javascript
loadData()
    .then(res => displayData(res))
    .catch(err => {
        console.error("Load failed:", err);
        showErrorMessage();
    });
```

---

## 22. Converting Callbacks to Promises (Promisify)

Sometimes you have old callback-based functions. You can wrap them in promises to use modern `.then()` or `async/await`.

### Promisifying setTimeout

```javascript
function delay(milliseconds) {
    return new Promise(resolve => {
        setTimeout(() => {
            resolve(`Waited ${milliseconds}ms`);
        }, milliseconds);
    });
}

delay(2000).then(message => console.log(message));
```

### Promisifying fs.readFile (Node.js)

```javascript
const fs = require("fs");

function readFilePromise(filePath) {
    return new Promise((resolve, reject) => {
        fs.readFile(filePath, "utf8", (error, data) => {
            if (error) reject(error);
            else resolve(data);
        });
    });
}

readFilePromise("myfile.txt")
    .then(content => console.log(content))
    .catch(err => console.log("Read error:", err));
```

### Quick method with util.promisify

```javascript
const fs = require("fs");
const { promisify } = require("util");

const readFile = promisify(fs.readFile);

readFile("myfile.txt", "utf8")
    .then(data => console.log(data))
    .catch(err => console.error(err));
```

---

## 23. Promise Utility Methods

### Promise.all()

Runs all promises in parallel. Waits for everyone to finish. If one fails, the whole thing fails.

```javascript
Promise.all([promise1, promise2, promise3])
    .then(results => console.log(results))
    .catch(error => console.error(error));
```

---

### Promise.allSettled()

Like `Promise.all()` but doesn't fail fast. Waits for all promises regardless of success or failure, then gives you all results.

```javascript
Promise.allSettled([promise1, promise2])
    .then(results => console.log(results));
```

---

### Promise.any()

Returns the first promise that succeeds. Ignores rejections unless all fail.

```javascript
Promise.any([promise1, promise2])
    .then(firstSuccess => console.log(firstSuccess));
```

---

### Promise.race()

Returns whichever promise finishes first, whether it succeeds or fails.

```javascript
Promise.race([promise1, promise2])
    .then(winner => console.log(winner));
```

---

### Promise.resolve() and Promise.reject()

Quick ways to create already-resolved or already-rejected promises.

```javascript
Promise.resolve("Success!");
Promise.reject("Failed!");
```

---
