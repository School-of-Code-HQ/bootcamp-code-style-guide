# JavaScript Coding Standards Guide

A comprehensive guide for writing clean, maintainable JavaScript code, with a focus on helping beginners develop good habits while providing professional standards.

## Table of Contents

1. [Code Organization](#code-organization)
2. [Variables and Naming](#variables-and-naming)
3. [Semi-colons and Syntax](#semi-colons-and-syntax)
4. [Functions](#functions)
5. [Objects and Arrays](#objects-and-arrays)
6. [Conditionals and Loops](#conditionals-and-loops)
7. [Error Handling](#error-handling)
8. [Common Bugs and Pitfalls](#common-bugs-and-pitfalls)
9. [Performance Basics](#performance-basics)
10. [Debugging](#debugging)

## Code Organization

Structure your code in a logical order for better readability and maintenance:

```javascript
// 1. Imports
import { formatDate } from './utils';
import { UserCard } from './components';

// 2. Constants/Configuration
const MAX_ATTEMPTS = 3;
const DEFAULT_SETTINGS = {
    theme: 'light',
    fontSize: 16,
};

// 3. Helper Functions
function validateInput(input) {
    return input.length >= 3;
}

// 4. Main Logic
function processUserData(userData) {
    if (!validateInput(userData.name)) {
        return null;
    }
    
    return {
        name: userData.name.toUpperCase(),
        date: formatDate(userData.date),
    };
}

// 5. Exports
export default processUserData;
```

## Variables and Naming

### Variable Declarations

Use `const` for values that won't be reassigned, `let` for values that will. Never use `var`.

Bad:
```javascript
var x = 5;
let CONSTANT_VALUE = 42;
const counter = 1;
counter++;  // Error!
```

Good:
```javascript
const MAX_VALUE = 42;
let counter = 1;
counter++;  // OK
```

### Naming Conventions

Names should be descriptive and indicate purpose:

1. **Boolean Variables** - Prefix with `is`, `has`, `can`, or similar:
```javascript
// Bad
let active = true;
let child = false;
let subscription = true;

// Good
let isActive = true;
let isChild = false;
let hasActiveSubscription = true;
let canEdit = true;
let shouldReload = false;
```

2. **Functions** - Use verb + noun combinations:
```javascript
// Bad
function data() {}
function user() {}

// Good
function getData() {}
function validateUser() {}
function setUserPreferences() {}
function isValidEmail() {}
```

3. **Arrays** - Use plural nouns:
```javascript
// Bad
const number = [1, 2, 3];
const fruit = ['apple', 'banana'];

// Good
const numbers = [1, 2, 3];
const fruits = ['apple', 'banana'];
```

4. **Classes** - Use PascalCase:
```javascript
// Bad
class userProfile {}
class dataHandler {}

// Good
class UserProfile {}
class DataHandler {}
```

## Semi-colons and Syntax

Always use semi-colons to clearly mark statement endings. While JavaScript has Automatic Semi-colon Insertion (ASI), relying on it can lead to bugs.

```javascript
// Bad - relying on ASI
let name = 'John'
let greeting = 'Hello'
[1, 2, 3].forEach(num => console.log(num))  // Can cause errors!

// Good
let name = 'John';
let greeting = 'Hello';
[1, 2, 3].forEach(num => console.log(num));
```

### Spacing and Indentation

Use consistent spacing (2 or 4 spaces) and maintain clean formatting:

```javascript
// Bad
function calculate(x,y){
if(x>0){return x+y}
else{return y}}

// Good
function calculate(x, y) {
    if (x > 0) {
        return x + y;
    } else {
        return y;
    }
}
```

## Functions

Keep functions focused on a single task. Use clear names that describe what the function does.

```javascript
// Bad
function doStuff(x) {
    console.log(x);
    document.title = 'New Page';
    return x + 5;
}

// Good
function calculateTotal(amount) {
    return amount * TAX_RATE + SHIPPING_FEE;
}

// Arrow functions for simple operations
const multiply = (x, y) => x * y;
const double = x => x * 2;

// Complex functions should be broken down
function updateUserProfile(userId, newData) {
    validateData(newData);
    saveToDatabase(userId, newData);
    notifyUser(userId);
}
```

## Objects and Arrays

### Object Creation and Access

```javascript
// Bad
const user = {name:'John',age:30};

// Good
const user = {
    name: 'John',
    age: 30,
};

// Use object destructuring
const { name, age } = user;

// Default values with destructuring
const { theme = 'light', fontSize = 16 } = userPreferences;
```

### Array Operations

Using array methods can make your code more readable and concise. Here are some common operations:

```javascript
// Bad - traditional for loop
for (let i = 0; i < array.length; i++) {
    console.log(array[i]);
}

// Better - for of loop
for (const item of array) {
    console.log(item);
}

// Best - using an array method like forEach with a named function
function logItem(item) {
    console.log(item);
}

array.forEach(logItem); // declarative, and easy to understand in english from this single line
```

Array methods like `forEach`, `filter`, `map`, and `reduce` are powerful tools for working with arrays. It's recommended to use named functions where possible for better readability and maintainability:

- `forEach`: Executes a provided function once for each array element.
- `filter`: Creates a new array with all elements that pass the test implemented by the provided function.
- `map`: Creates a new array with the results of calling a provided function on every element in the calling array.
- `reduce`: Executes a reducer function on each element of the array, resulting in a single output value.

Examples:

```javascript
// Named function for filtering active items
function isActive(item) {
    return item.active;
}
const filteredItems = items.filter(isActive);

// Named function for mapping item names
function getItemName(item) {
    return item.name;
}
const itemNames = items.map(getItemName);

// Named function for reducing to a total price
function sumPrices(sum, item) {
    return sum + item.price;
}
const total = items.reduce(sumPrices, 0);
```

### Trailing Commas

Trailing commas are recommended for cleaner version control diffs and easier reordering:

```javascript
const config = {
    host: 'localhost',
    port: 3000,
    debug: true,  // Trailing comma
};

const colors = [
    'red',
    'green',
    'blue',  // Trailing comma
];
```

## Conditionals and Loops

### Clean Conditionals

```javascript
// Bad
if (user.age >= 18 && user.country === 'US' && !user.isRestricted) {
    // Do something
}

// Good
const isAdult = user.age >= 18;
const isUSResident = user.country === 'US';
const isAllowed = !user.isRestricted;

if (isAdult && isUSResident && isAllowed) {
    // Do something
}
```

### Modern Loop Approaches

```javascript
// Bad
for (let i = 0; i < items.length; i++) {
    if (items[i].value < 10) {
        processItem(items[i]);
    }
}

// Good
items
    .filter(item => item.value < 10)
    .forEach(processItem);

// Using for...of for simple iteration
for (const item of items) {
    console.log(item);
}
```

## Error Handling

Always handle potential errors, especially in async operations:

```javascript
// Bad
async function getData() {
    const response = await fetch(url);
    return response.json();
}

// Good
async function getData() {
    try {
        const response = await fetch(url);
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        return await response.json();
    } catch (error) {
        console.error('Error fetching data:', error);
        throw error;  // or return fallback data
    }
}
```

## Common Bugs and Pitfalls

### Type Comparison

```javascript
// Bad - loose equality
if (userInput == 123) {}  // '123' == 123 is true

// Good - strict equality
if (userInput === 123) {}  // '123' === 123 is false
```

### Reference vs Copy

```javascript
// Bad - creates a reference
const newArray = oldArray;
const newObject = oldObject;

// Good - creates a (shallow) copy
const newArray = [...oldArray];
const newObject = { ...oldObject };
```

### Variable Scope

```javascript
// Bad
var x = 1;
if (true) {
    var x = 2;  // Same variable!
}

// Still bad, but better
let x = 1;
if (true) {
    let x = 2;  // Different variable
}
```

The best solution would be to use different variable names to avoid confusion in the first place.

## Performance Basics

### Avoid Unnecessary Computation

```javascript
// Bad - multiple iterations
const result = numbers
    .filter(n => n > 2)
    .map(n => n * 2);

// Good - single iteration
const result = numbers.reduce((acc, n) => {
    if (n > 2) {
        acc.push(n * 2);
    }
    return acc;
}, []);
```

### Function Reuse

```javascript
// Bad - creates new function each time
items.map(item => ({
    ...item,
    format: () => `${item.name} (${item.id})`
}));

// Good - reuse function
const formatItem = (name, id) => `${name} (${id})`;
items.map(item => ({
    ...item,
    format: formatItem
}));
```

## Debugging

### Console Logging

Use descriptive console logs to help debug your code:

```javascript
// Bad
console.log(x);
console.log('here');
console.log(1);

// Good
console.log('User Data:', userData);
console.log('Error in getUser function:', error);

// For multiple values, you can use object logging
console.log({
    userId,
    userEmail,
    timestamp
});

// Use console.table for arrays/objects
console.table(users);
```

Remember:

- These are guidelines, not strict rules
- Consistency within a project is most important
- Code should be readable and maintainable
- When in doubt, opt for clarity over cleverness