# Scope

## Overview
In this lesson, we'll introduce the concept of scope in JavaScript and cover some of the common pitfalls.

## Objectives
1. Explain in general terms what the execution context is.
2. Describe the difference between global- and function-scoped code.
3. Understand how block scoping affects variables declared with `let` and `const`.

## Introduction
Scope is a ubiquitous concept in programming and one of the most misunderstood principles in JavaScript, frustrating even seasoned engineers. Not understanding how scopes work will lead to pain. Just ask this guy:
![Telescope fail](https://user-images.githubusercontent.com/17556281/29201150-b9b06716-7e29-11e7-90ad-cb4065d0d9ff.gif)

## Let's talk about Slack, baby
As the newest engineer at Flatbook, you have access to the company's Slack team. The Slack team is organized into channels, some of which are company-wide, such as the main `#general` channel, and some of which are used by individual teams for intra-team communication, such as `#education`, `#engineering`, and `#marketing`.

Each channel forms its own _scope_, meaning that its messages are only visible to those with access to the channel. Within `#engineering`, you can interact with the other members of your team, referring and responding to messages that they send. However, you can't see any of the messages posted inside `#marketing` — that's a different scope that you don't have access to.

The same exact principle of distinct scopes exists in JavaScript, and it has to do with where declared variables and functions are visible.

## Execution contexts
Just as every message on Slack is sent in a channel, every piece of JavaScript code is run in an _execution context_ (EC). In a Slack channel, we have access to all of the messages sent in that channel; we can send a message that references any of the other messages posted in the same channel. Similarly, in a JavaScript execution context, we have access to all of the variables and functions declared in that EC. Within an execution context, we can write an expression that references a variable or invokes a function declared in the same EC.

![Execution context and scope](https://user-images.githubusercontent.com/17556281/29332359-c0a8280e-81cd-11e7-90fc-0dd1cd23a48b.png)

Up to this point, almost all of the JavaScript code we've written has lived in the _global execution context_, the context that implicitly wraps all of the JavaScript code in a project. Variables and functions declared in the global execution context — in the _global scope_ — are accessible everywhere in your JavaScript code:
```js
// 'myFunc' is declared in the global scope and available everywhere in your code:
function myFunc () {
  return 42;
}
// => undefined

// 'myVar' is able to reference and invoke 'myFunc' because both are declared in the same scope (the global execution context):
const myVar = myFunc() * 2;
// => undefined

myVar;
// => 84
```

![Execution context and scope](https://user-images.githubusercontent.com/17556281/29332361-c0b0494e-81cd-11e7-8d6c-e803ab5a9b62.png)

***Top Tip***: If a variable or function is **not** declared inside a function or block, it's in the global execution context.

## Function scope
When we declare a new function and write some code in the function body, we're no longer in the global execution context. The function creates a new execution context with its own scope. Inside the function body, we can reference variables and functions declared in the function's scope:
```js
function myFunc () {
  const myVar = 42;

  return myVar * 2;
}
// => undefined

myFunc();
// => 84
```

However, from outside the function, we can't reference anything declared inside of it:
```js
function myFunc () {
  const myVar = 42;
}
// => undefined

myVar * 2;
// Uncaught ReferenceError: myVar is not defined
```

The function body creates its own scope. It's like a separate channel on Slack — only the members of that channel can read the messages sent in it.

## Block scope
A block statement also creates its own scope... kind of. As of ES2015, JavaScript has partial support for block scoping.

### Functions
Functions declared inside of a block are **not** block-scoped:
```js
if (true) {
  function myFunc () {
    return 42;
  }
}

myFunc();
// => 42
```

However, be careful about declaring functions inside of conditional blocks. If the condition in the above example were falsy, the block would never run and the function would never be declared. The subsequent invocation of `myFunc()` would result in an error:
```js
if (false) {
  function myFunc () {
    return 42;
  }
}

myFunc();
// ERROR: Uncaught TypeError: myFunc is not a function
```

### Variables
Variables declared with `var` are **not** block-scoped:
```js
if (true) {
  var myVar = 42;
}

myVar;
// => 42
```

However, variables declared with `const` and `let` **are** block-scoped:
```js
if (true) {
  const myVar = 42;

  let myOtherVar = 9001;
}

myVar;
// Uncaught ReferenceError: myVar is not defined

myOtherVar;
// Uncaught ReferenceError: myOtherVar is not defined
```

This is yet another reason to **never use `var`**. As long as you stick to declaring variables with `const` and `let`, what happens in block stays in block.

![What happens here stays here](https://user-images.githubusercontent.com/17556281/29347999-6461aed4-821e-11e7-9f83-13d0b9e88947.gif)

## Resources
- MDN
  + [Scope](https://developer.mozilla.org/en-US/docs/Glossary/scope)
  + [Functions — Function scope](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions#Function_scope)
