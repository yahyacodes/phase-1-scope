# Introduction to Scope

## Learning Goals

- Explain in general terms what the execution context is.
- Describe the difference between global- and function-scoped code.
- Understand how block scoping affects variables declared with `let` and `const`.

## Introduction

_Scope_ , in short, is the concept of **where something is available**. In the
case of JavaScript, it has to do with where declared variables and methods are
available within our code.

Scopes can also be layered in a hierarchy, so that child scopes have access to
parent scopes, but not vice versa.

Scope is a present concept in programming and one of the most misunderstood
principles in JavaScript, frustrating even seasoned engineers. Not understanding
how scope works will lead to pain.

## Let's talk about Slack

As the newest engineer at Flatbook, you have access to the company's Slack team.
The Slack team is organized into channels, some of which are company-wide, such
as the main `#general` channel, and some of which are used by individual teams
for intra-team communication, such as `#education`, `#engineering`, and
`#marketing`.

Each channel forms its own _scope_, meaning that its messages are only visible
to those with access to the channel. Within `#engineering`, you can interact
with the other members of your team, referring and responding to messages that
they send. However, you can't see any of the messages posted inside `#marketing`
— that's a different scope that you don't have access to.

The same exact principle of distinct scopes exists in JavaScript, and it has to
do with where declared variables and functions are visible.

## Execution contexts

Just as every message on Slack is sent in a channel, every piece of JavaScript
code is run in an _execution context_. In a Slack channel, we have access to all
of the messages sent in that channel; we can send a message that references any
of the other messages posted in the same channel. Similarly, in a JavaScript
execution context, we have access to all of the variables and functions declared
in that context. Within an execution context, we can write an expression that
references a variable or invokes a function declared in the same context.

In JavaScript, scope refers to the context in which variables and functions are
declared and accessed. It determines the visibility and accessibility of variables
and functions in different parts of your code. Understanding scope is crucial for
writing maintainable and error-free JavaScript programs.

## JavaScript has the following kinds of scopes:

- Global scope: The default scope for all code running in script mode.

> > The following code is valid due to the variable being declared
> > outside the function, making it global:

```js
const x = "declared outside function";

exampleFunction();

function exampleFunction() {
  console.log("Inside function");
  console.log(x);
}

console.log("Outside function");
console.log(x);
```

- Function scope: The scope created with a function.

Variables defined inside a function cannot be accessed from anywhere outside
the function, because the variable is defined only in the scope of the function.
However, a function can access all variables and functions defined inside the
scope in which it is defined.

```js
// The following variables are defined in the global scope
const num1 = 20;
const num2 = 3;
const name = "Chamakh";

// This function is defined in the global scope
function multiply() {
  return num1 * num2;
}

console.log(multiply()); // 60

// A nested function example
function getScore() {
  const num1 = 2;
  const num2 = 3;

  function add() {
    return `${name} scored ${num1 + num2}`;
  }

  return add();
}

console.log(getScore()); // "Chamakh scored 5"
```

**_Top Tip_**: If a variable or function is **not** declared inside a function
or block, it's in the global execution context.

> > However, from outside the function, we can't reference anything declared inside of it:

```js
function getScore() {
  const num1 = 2;
  const num2 = 3;
}
// => undefined

num1 * num2;
// Uncaught ReferenceError: myVar is not defined
```

> > The function body creates its own scope. It's like a separate channel on Slack
> > — only the members of that channel can read the messages sent in it.

> > In addition, variables declared with **let** or **const** can belong to an additional scope:

- Block scope: The scope created with a pair of curly braces (a block).

**Block scope** is a type of scope in JavaScript that defines the **visibility** and **accessibility** of
**variables** and **functions** within a specific block of code. A block of code is typically enclosed
within curly braces **{}**. Variables declared with **let** and **const**, introduced in **ECMAScript 6 (ES6)**,
have block scope, <!-- meaning they are only accessible within the block where they are defined. -->

- Variables declared with `const` and `let` **are** block-scoped:

```js
if (true) {
  let blockScopedVar = 42; // blockScopedVar has block scope
  console.log(blockScopedVar); // This works
}

console.log(blockScopedVar); // This will result in an error because
// blockScopedVar is not defined in the outer scope
```

In this example, **blockScopedVar** is declared within the **if** block, so it has **block scope** and is only
accessible within that block. Attempting to access it outside the block will result in an error.

- Variables declared with `var` are **not** block-scoped:

```js
if (true) {
  var varVariable = 42; // This is not block-scope
}

varVariable; // This also works because varVariable
// is hoisted to the function or global scope
```

This is yet another reason to **never use `var`**. As long as you stick to
declaring variables with `const` and `let`, what happens in block stays in
block.

## The global gotcha

The _global gotcha_ in JavaScript refers to a **common issue** that developers may encounter
when working with <!-- global variables or accidentally creating global variables unintentionally. -->
This can lead to unexpected and hard-to-debug behavior in your code.

```js
globalVar = 42;
// This creates a global variable unintentionally
```

Variables created without a `const`, `let`, or `var` keyword are **always globally-scoped**, regardless
of where they sit in your code. If you create one inside of a block, it's still available globally:

```js
function setGlobalVariable() {
  globalVar = 42;
  // This creates a global variable unintentionally
}

setGlobalVariable();
globalVar; // Outputs 42
```

If you create one inside of a function — wait for it — it's still available
globally:

```js
function bankAccount() {
  secretPassword = "il0v3pupp135";

  return "bankAccount() function invoked!";
}

bankAccount();
// => "bankAccount() function invoked!"

secretPassword;
// => "il0v3pupp135"
```

The global variable named **secretPassword** was accidentally created within the
`bankAccount` function due to a missing `var`, `let`, or `const` keyword when declaring it. This resulted
in `secretPassword` becoming accessible from **anywhere** in the code, even outside the function. Consequently,
after invoking `bankAccount()`, the variable retained the value `il0v3pupp135` and could be accessed globally,
potentially leading to unintended side effects and naming conflicts. It's a best practice to explicitly scope
variables using `var`, `let`, or `const` to prevent accidental \*\*global variable\*\* creation and maintain code predictability.

## Conclusion

Scope in programming defines the visibility and accessibility of variables and functions within code.
In JavaScript, there are mainly three types of scope:

1. Global Scope: Variables declared outside of functions are in the global scope and are accessible from anywhere in the code.

1. Function Scope: Variables declared inside a function are in function scope and are accessible only within that function.

1. Block Scope: Introduced with let and const in ES6, variables declared within blocks (e.g., within curly braces {}) have block scope, limiting their visibility to that block.

Understanding scope is crucial for managing variable lifetimes and preventing naming conflicts. Properly scoped
variables and functions enhance code **readability** and **maintainability** by encapsulating functionality and
limiting access. So, to sum up our tricks for taming the scope monster:

1. Always use `const` and `let` to declare variables.
2. Keep in mind that every function creates its own scope, and any variables or
   functions you declare inside of the function will not be available outside of
   it.
3. For Dijkstra's sake, **_always use `const` and `let` to declare variables_**.

## Resources

- MDN
  - [Scope](https://developer.mozilla.org/en-US/docs/Glossary/scope)
  - [Functions — Function scope](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions#Function_scope)
- W3Schools
  - [JavaScript-Scope](https://www.w3schools.com/js/js_scope.asp)
- FreeCodeCamp
  - [Scope-and-Closures-in-javaScript](https://www.freecodecamp.org/news/scope-and-closures-in-javascript/)
