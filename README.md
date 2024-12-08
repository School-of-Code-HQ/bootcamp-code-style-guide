# JavaScript Coding Standards Guide

A comprehensive guide for writing clean, maintainable JavaScript code, with a focus on helping beginners develop good habits and adhere to professional standards. This guide doesn’t just tell you how to do things—it also provides the why behind each recommendation, so you can make informed choices as you grow as a developer.

**Note:** While these guidelines represent common industry practices, they aren’t absolute rules. Different teams or projects may follow slightly different conventions. The conversation about these types of decisions is the important part. The key is consistency and clarity.

## Table of Contents

1. [Code Organization](#code-organization)
1. [Variables and Naming](#variables-and-naming)
1. [Semi-colons and Syntax](#semi-colons-and-syntax)
1. [Functions](#functions)
1. [Objects and Arrays](#objects-and-arrays)
1. [Conditionals and Loops](#conditionals-and-loops)
1. [Error Handling](#error-handling)
1. [Common Bugs and Pitfalls](#common-bugs-and-pitfalls)
1. [Debugging](#debugging)

## Code Organization

**Goal:** Make your code easy to navigate, read, and maintain. Grouping related elements together in a logical order helps everyone (including your future self) understand the flow.

**Recommended Order:**

1. **Imports** – Bring in external modules/functions at the top.
2. **Constants/Configuration** – Declare fixed values and configuration options.
3. **Helper Functions** – Define small, reusable functions that support your main logic.
4. **Main Logic** – Implement your core functionality or “business logic.”
5. **Exports** – Export functions or objects so other modules can use them.

**Example:**

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

**Why this helps:** By following a consistent structure, it’s easier to find what you need. It also encourages separation of concerns: helper functions don’t get mixed up with core logic, making the codebase more modular and testable.

## Variables and Naming

**Goal:** Use meaningful, consistent naming so that code is self-explanatory.

### Variable Declarations

- Use `const` for values that never change.
- Use `let` for values that will change.
- Avoid `var` as it can create confusing scoping issues.

**Examples:**

```javascript
// Bad
var x = 5;  // 'var' can cause unexpected behavior
let CONSTANT_VALUE = 42; // Upper-case implies it never changes, so use const
const counter = 1;
counter++; // Error: Assignment to constant variable

// Good
const MAX_VALUE = 42; // Clear, never changes
let counter = 1;      // Clearly can change later
counter++;            // Allowed, since it's a let
```

### Naming Conventions

**Descriptive Names:** Give variables descriptive names that indicate their purpose. Avoid single letters (e.g., x) unless it’s a very short scope like a loop index.

- **Boolean Variables:** Start with `is`, `has`, `can`, `should` to indicate true/false meaning.

```javascript
// Bad
let active = true;
let child = false;
let subscription = true;
let edit = true;
let reload = false;

// Good
let isActive = true;
let isChild = false;
let hasActiveSubscription = true;
let canEdit = true;
let shouldReload = false;
```

- **Functions:** Use verb + noun combinations for clarity.

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

- **Arrays:** Use plural nouns to indicate multiple items.

```javascript
// Bad
const number = [1, 2, 3];
const fruit = ['apple', 'banana'];

// Good
const numbers = [1, 2, 3];
const fruits = ['apple', 'banana'];
```

- **Classes:** Use PascalCase.

```javascript
// Bad
class userProfile {}
class dataHandler {}

// Good
class UserProfile {}
class DataHandler {}
```

**Why this helps:** Meaningful, consistent names save mental effort. When someone reads your code, they shouldn’t have to guess what a variable or function does.

## Semi-colons and Syntax

**Goal:** Write syntactically clear code to avoid confusing bugs.

- Always use semi-colons to mark the end of statements. JavaScript’s Automatic Semicolon Insertion can cause unexpected errors.

```javascript
// Bad - relying on automatic semicolon insertion
let name = 'John'
let greeting = 'Hello'
[1, 2, 3].forEach(num => console.log(num))  // Can cause errors!

// Good
let name = 'John';
let greeting = 'Hello';
[1, 2, 3].forEach(num => console.log(num));
```

### Spacing and Indentation

- Choose a consistent indentation (2 or 4 spaces) and stick with it.
- Add spaces around operators and after commas for readability.

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

**Why this helps:** Consistent formatting isn’t just aesthetic—it reduces cognitive load and makes code easier to navigate, review, and maintain.

## Functions

**Goal:** Write functions that do one thing well.

- Keep functions focused on a single task.
- Give functions clear names that describe their purpose.
- Break larger tasks into smaller helper functions.

```javascript
// Bad: does multiple unrelated things
function doStuff(x) {
    console.log(x);
    document.title = 'New Page';
    return x + 5;
}

// Good: focused function
function calculateTotal(amount) {
    return amount * TAX_RATE + SHIPPING_FEE;
}

// You can use arrow functions for simple operations
const multiply = (x, y) => x * y;
const double = x => x * 2;

// Break down complex logic
function updateUserProfile(userId, newData) {
    validateData(newData);
    saveToDatabase(userId, newData);
    notifyUser(userId);
}
```

**Why this helps:** Smaller, well-named functions are easier to understand, test, and reuse. It’s simpler to fix or improve parts of your code without breaking something else.

## Objects and Arrays

**Goal:** Use clean, modern patterns for data structures.

### Object Creation and Access

```javascript
// Bad
const user = {name:'John',age:30};

// Good
const user = {
    name: 'John',
    age: 30,
};

// Destructure for convenience
const { name, age } = user;
```

### Default Values

```javascript
const { theme = 'light', fontSize = 16 } = userPreferences;
```

### Array Operations

Instead of traditional loops, prefer more readable approaches:

```javascript
// Bad - traditional loop
for (let i = 0; i < fruits.length; i++) {
    console.log(fruits[i]);
}

// Good - modern approaches
for (const fruit of fruits) {
    console.log(fruit);
}

// Even better - array methods
fruits.forEach(fruit => console.log(fruit));

// Even better still - named functions
function printFruit(fruit) {
    console.log(fruit);
}

fruits.forEach(printFruit); // declarative and readable in plain english
```

Use array methods like `forEach`, `filter`, `map`, `reduce` for common tasks. Named functions can improve readability:

```javascript
function isActive(item) { 
    return item.active;
}

function getItemName(item) { 
    return item.name;
}

const activeItems = items.filter(isActive);
const itemNames = items.map(getItemName);
```

**Why this helps:** Modern array methods and object destructuring make code more expressive, reducing boilerplate and clarifying intent.

### Trailing Commas

Use trailing commas in multi-line objects and arrays. It makes adding or removing lines easier and reduces syntax errors.

```javascript
const config = {
    host: 'localhost',
    port: 3000,
    debug: true,
};
```

## Conditionals and Loops

**Goal:** Make logic clear and easy to follow.

### Clean Conditionals

Break down complex conditions into readable parts:

```javascript
// Bad
if (user.age >= 18 && user.country === 'US' && !user.isRestricted) {
    console.log("Welcome!");
}

// Good
const isAdult = user.age >= 18;
const isUSResident = user.country === 'US';
const isAllowed = !user.isRestricted;

if (isAdult && isUSResident && isAllowed) {
    console.log("Welcome!");
}
```

### Modern Loop Approaches

Leverage array methods where you can to make code more declarative:

```javascript
// Bad
for (let i = 0; i < items.length; i++) {
    if (items[i].value < 10) {
        processItem(items[i]);
    }
}

// Good
function isItemValueUnder10(item) {
    return item.value < 10;
}

items
    .filter(isItemValueUnder10)
    .forEach(processItem);
```

**Why this helps:** Clear conditions and loops help you and others quickly understand what the code is meant to do.

## Error Handling

**Goal:** Gracefully handle errors so your app doesn’t crash unexpectedly.

**Example with try/catch:**

```javascript
// Bad
function parseJSON(str) {
    return JSON.parse(str);
}

// Good
function parseJSON(str) {
    try {
        return JSON.parse(str);
    } catch (error) {
        console.error(`Error parsing JSON: ${error.message}`, error);
        throw new Error('Invalid JSON format');
    }
}
```

**Why this helps:** Good error handling makes your code more resilient, easier to debug, and more reliable in production.

## Common Bugs and Pitfalls

### Strict Equality

Use `===` and `!==` instead of `==` and `!=` to avoid type coercion surprises

```javascript
// Bad
if (shouldContinue == true) {
    proceed();
}

// Good
if (shouldContinue === true) {
    proceed();
}
```

**Why this helps:** Using strict equality avoids unexpected type coercion, making your code more predictable and less error-prone.

### Reference vs Copy

Know the difference between referencing and copying objects or arrays.

```javascript
// Shallow copy arrays/objects
const newArray = [...oldArray];
const newObject = { ...oldObject };
```

### Variable Scope

Avoid `var` to prevent scope-related bugs. Stick to `let` and `const`.

```javascript
// Bad
for (var i = 0; i < 5; i++) {
    console.log(i);
}
console.log(i); // 5

// Good
for (let i = 0; i < 5; i++) {
    console.log(i);
}
console.log(i); // ReferenceError: i is not defined
```

**Why this helps:** Understanding these pitfalls saves you time and frustration by preventing common errors.

## Debugging

**Goal:** Make finding and fixing issues easier.

- Use descriptive `console.log` messages.
- Use `console.table` for arrays/objects.
- Provide context in your logs so you know where they come from.

```javascript
console.log('User Data:', userData);
console.table(users);
```

**Why this helps:** Good debugging practices let you quickly pinpoint issues without guesswork.

## Summary and Tools

- **Consistency is Key:** Whichever conventions you pick, apply them consistently.
- **Focus on Readability:** Write code for humans first—other developers (and future you) will thank you.
- **Tools Help:** Use linters (like ESLint) and formatters (like Prettier) to automate style checks and formatting.

Over time, you’ll develop your own style and understand when to follow these conventions strictly and when to adapt them. As a beginner, these guidelines form a solid foundation for producing professional, maintainable code.
