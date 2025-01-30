# JavaScript Advanced - Week 2

## Table of Contents
- [Day 1: JavaScript Fundamentals](#day-1-javascript-fundamentals)
- [Day 2: Error Handling and Call Stack](#day-2-error-handling-and-call-stack)
- [Day 3: Advanced JavaScript Concepts](#day-3-advanced-javascript-concepts)
- [Day 4: Objects and Classes](#day-4-objects-and-classes)
- [Day 5: Advanced Functions](#day-5-advanced-functions)
- [Day 6: Asynchronous JavaScript](#day-6-asynchronous-javascript)

## Day 1: JavaScript Fundamentals

### Synchronous vs Asynchronous Code Execution

#### Description
Understanding the difference between synchronous and asynchronous code execution in JavaScript.

#### Example Code

```javascript
// Synchronous
console.log("Start");
function synchronousTask() {
    return "Task Complete";
}
console.log(synchronousTask());
console.log("End");

// Asynchronous
console.log("Start");
setTimeout(() => {
    console.log("Async Task Complete");
}, 1000);
console.log("End");
```

### Execution Context

#### Description
Understanding how JavaScript manages scope and variable access.

#### Example Code

```javascript
let globalVariable = "I'm global";

function outerFunction() {
    let outerVariable = "I'm from outer";
    
    function innerFunction() {
        let innerVariable = "I'm from inner";
        console.log(innerVariable, outerVariable, globalVariable);
    }
    
    innerFunction();
}

outerFunction();
```

### Strict Mode

#### Description
Implementing strict mode and understanding its implications.

#### Example Code

```javascript
"use strict";

function strictExample() {
    let obj = {};
    Object.defineProperty(obj, "prop", {
        value: 42,
        writable: false
    });
    
    try {
        obj.prop = 100;
    } catch (error) {
        console.error("Cannot modify read-only property");
    }
}
```

## Day 2: Error Handling and Call Stack

### Call Stack Demonstration

#### Description
Understanding how JavaScript manages function calls.

#### Example Code

```javascript
function firstFunction() {
    console.log("Inside first function");
    secondFunction();
}

function secondFunction() {
    console.log("Inside second function");
    thirdFunction();
}

function thirdFunction() {
    console.log("Inside third function");
    console.trace();
}
```

### Exception Handling

#### Description
Implementing proper error handling in JavaScript.

#### Example Code

```javascript
function divideNumbers(a, b) {
    try {
        if (b === 0) {
            throw new Error("Division by zero!");
        }
        return a / b;
    } catch (error) {
        console.error("Error:", error.message);
        return null;
    } finally {
        console.log("Division operation attempted");
    }
}
```

### Hoisting

#### Description
Understanding variable and function hoisting in JavaScript.

#### Example Code

```javascript
console.log(hoistedVar); // undefined
var hoistedVar = "I'm hoisted";

hoistedFunction(); // Works
function hoistedFunction() {
    console.log("I'm a hoisted function");
}
```

## Day 3: Advanced JavaScript Concepts

### Temporal Dead Zone

#### Description
Understanding the temporal dead zone (TDZ) in JavaScript.

#### Example Code

```javascript
// This is in the TDZ for letVariable
try {
    console.log(letVariable);
} catch (error) {
    console.log("TDZ in action!");
}

let letVariable = "I'm a let";
```

### Shadowing and Illegal Shadowing

#### Description
Understanding variable shadowing and its limitations.

#### Example Code

```javascript
let globalVar = "Global";

function shadowExample() {
    let globalVar = "Shadowed"; // Legal shadowing
    console.log(globalVar);
    
    {
        let globalVar = "Block Scoped"; // Legal shadowing
        console.log(globalVar);
    }
}
```

### Coercion and Memory Leaks

#### Description
Understanding type coercion and common memory leak patterns.

#### Example Code

```javascript
// Type Coercion
console.log("5" + 3);  // "53"
console.log("5" - 3);  // 2

// Memory Leak Example
let leakyArray = [];

function causeMemoryLeak() {
    let largeData = new Array(1000000);
    leakyArray.push(largeData);
}
```

## Day 4: Objects and Classes

### Stack Overflow Example

#### Description
Understanding stack overflow scenarios.

#### Example Code

```javascript
function recursiveFunction(n) {
    try {
        return recursiveFunction(n + 1);
    } catch (error) {
        console.log("Stack overflow occurred!");
        return n;
    }
}
```

### Deep Copy vs Shallow Copy

#### Description
Understanding different types of object copying.

#### Example Code

```javascript
const original = {
    name: "John",
    details: {
        age: 30,
        address: {
            city: "New York"
        }
    }
};

// Shallow Copy
const shallowCopy = { ...original };

// Deep Copy
const deepCopy = JSON.parse(JSON.stringify(original));
```

### Memoization

#### Description
Implementing function memoization for performance optimization.

#### Example Code

```javascript
function memoize(fn) {
    const cache = new Map();
    
    return function (...args) {
        const key = JSON.stringify(args);
        if (cache.has(key)) {
            return cache.get(key);
        }
        
        const result = fn.apply(this, args);
        cache.set(key, result);
        return result;
    };
}
```

## Day 5: Advanced Functions

### Constructors

#### Description
Understanding constructor functions in JavaScript.

#### Example Code

```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
    
    if (!new.target) {
        throw new Error("Must be called with new");
    }
}
```

### Generator Functions

#### Description
Understanding generator functions and their use cases.

#### Example Code

```javascript
function* numberGenerator() {
    yield 1;
    yield 2;
    yield 3;
}

const gen = numberGenerator();
```

### Function Currying

#### Description
Implementing function currying in JavaScript.

#### Example Code

```javascript
function curry(fn) {
    return function curried(...args) {
        if (args.length >= fn.length) {
            return fn.apply(this, args);
        }
        return function(...moreArgs) {
            return curried.apply(this, args.concat(moreArgs));
        };
    };
}
```

## Day 6: Asynchronous JavaScript

### Promises

#### Description
Understanding and implementing Promises.

#### Example Code

```javascript
function delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

delay(1000)
    .then(() => console.log("Delayed for 1 second"))
    .catch(error => console.error(error));
```

### Fetch API

#### Description
Working with the Fetch API for HTTP requests.

#### Example Code

```javascript
async function fetchData() {
    try {
        const response = await fetch('https://api.example.com/data');
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        const data = await response.json();
        return data;
    } catch (error) {
        console.error('Error:', error);
    }
}
```

### Async/Await

#### Description
Using async/await for asynchronous operations.

#### Example Code

```javascript
async function processDataInSequence() {
    try {
        const result1 = await fetchFirstData();
        const result2 = await processResult(result1);
        const result3 = await finalizeData(result2);
        
        return result3;
    } catch (error) {
        console.error('Error in sequence:', error);
        throw error;
    }
}
```

## Contributing
Feel free to submit issues and enhancement requests.

## License
This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.