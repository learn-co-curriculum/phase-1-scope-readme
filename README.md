# Introduction to Scope

## Learning Goals

- one
- two
- three

## Introduction

The more complex your code becomes, the more important it is to pay attention to
scope. So, what exactly is it? _Scope_ is the concept of **where something is
available**. In the case of JavaScript, it has to do with where variables and
functions are accessible within our code.

Scope is not something fully tangible, there is no specific syntax to it. As a
result, it can be a difficult concept to grasp. However, scope affects
everything you write. All code you have written thus far has used scope in some
way, even without you knowing. Consequently, it is important that you understand
what it is and how it impacts the code you write.

In this lesson, we will dissect what scope is and break it down into its three
main levels in JavaScript:

1. Global scope
1. Function scope
1. Block scope

## Scope

One thing to understand before scope can begin to make sense is: **we are not
always able to access variables or functions wherever we want**. For example, a
variable defined within one function may not be directly accessible in a
separate function. This is where scope comes in.

Scope determines if and where variables and functions are accessible by other
parts of the code. It allows you to keep certain parts of your code separate
from one another.

Take the following snippet as an example:

```js
const parkingLot = true;

function employee() {
  const enterBuilding = true;
  console.log(parkingLot, enterBuilding);
}

employee(); // => true true
console.log(parkingLot); // => true
console.log(enterBuilding); // => ReferenceError: enterBuilding is not defined
```

In this example, one variable (`parkingLot`) is defined _outside_ of the
`employee` function, while the other (`enterBuilding`) is defined _inside_ the
function.

When logging both variables _inside_ the `employee` function, it is successful.
The function has access to both of them.

However, when trying to log both variables _outside_ the function, the
`enterBuilding` variable throws an error. The code outside of the function does
not have access to that inside variable. It only has access to the outside
variable.

This example demonstrates two of the three different levels of scope in
JavaScript. After the next few sections, you should be able to come back to this
example and determine which two.

Let's cover those three scope levels now.

## Global Scope

Global scope is the **outermost scope**. Anything defined here is accessible
everywhere else in your code. This makes it the simplest scope to utilize as it
does not create any limitations on where you can access variables.

Anything defined at the top level of a file is considered to be definied
globally. In other words, **anything defined _outside_ of curly braces** `{}`
are globally scoped.

Let's take a look at some examples. Imagine the following is _all_ the code
inside a file:

```js
const accessibleAnywhere = true;

function globalFn() {
  return true;
}

console.log(accessibleAnywhere); // => true
globalFn(); // => true
```

Both the variable and function are defined at the top level of the file, making
them both available in the global scope. If we were to continue adding code to
this file, we could access both of them anywhere we want.

While global scope makes it easy to access variables and functions wherever and
whenever we want to, it is not always a good idea. In general, it's best
practice to make variables and functions available only where they're needed
— and nowhere else.

Making a variable available in places that shouldn't have access to it can lead
to bad things and make your debugging process more complex. The more pieces of
code that can access a given variable, the more places you have to check for
bugs if/when that variable contains an unexpected value.

This is where the other levels of scope comes in handy.

## Function Scope

Function scope is the **scope within a single function**. Anything **defined
inside the body of a function** is considered to be part of that function's
scope.

Anything defined in the function's scope can be accessed anywhere _within the
function's body_. For example:

```js
let count = 0;

function counter() {
  const resetValue = 100;
  console.log("Counter will reset after: ", resetValue);

  if (count < resetValue) {
    count++;
  } else {
    count = 0;
  }
}

counter(); // => Counter will reset after: 100
```

In the above example, the `resetValue` variable is defined within the `counter`
_function's scope_. We can see it's accessible anywhere within the function,
both in the `console.log` and the `if` statement.

The example also showcases again how `count`, a variable defined in the _global
scope_, is accessible anywhere in our code.

The reverse is not true, however. Variables defined in the _function scope_ are
**not** accessible anywhere outside of that scope. In other words, the global
scope cannot access the function scope.

Let's take a look at a modified version of our `counter` example to see that in
action:

```js
let count = 0;
function counter() {
  const resetValue = 100;
  console.log("Counter will reset after: ", resetValue);
}

counter(); // => Counter will reset after: 100
console.log(resetValue); // => ReferenceError: resetValue is not defined
```

The `resetValue` variable does not exist outside of its function's scope.
Anything outside of a function cannot reference anything defined within it.

## Block Scope

Block scope is the **scope within a pair of curly braces**. Anything defined
within a pair of `{}` are considered to be part of that block's scope. Function
scope may sometimes be considered the same as block scope, but here we are
specifically referring to other statements that use `{}`. For example, `if`
statements, or `for` loops.

As with function scope, anything defined in a block's scope can be accessed
anywhere _within the block's body_. Anything outside of the block can not access
anything inside.

This includes `if/else` blocks, for example:

```js
if (animal === "capybara") {
  const favoriteFruit = "yuzu";
  console.log("A capybara's favorite bath fruit is: ", favoriteFruit); // => A capybara's favorite bath fruit is: yuzu
} else {
  console.log(favoriteFruit); // => ReferenceError: favoriteFruit is not defined
  return "No animal here, sorry!";
}

console.log(favoriteFruit); // => ReferenceError: favoriteFruit is not defined
```

Even though the `if/else` blocks are part of the same conditional, each
statement has its own block scope. Both the `if` and `else` statement have their
own sets of curly braces `{}`, and therefore have separate scopes. Anything
defined within the `if` statement, such as `favoriteFruit`, does not exist
outside of that block.

## Gotcha's to Be Aware Of

Now that we've covered the main three scopes, there are two gotcha's you should
be aware of moving forward. Both have to do with how you declare a variable.

All of our examples thus far have either used `const` or `let` to declare a
variable. For best practices, we recommend you always use one of the two.
However, you may encounter legacy code that uses `var` or doesn't use a keyword
at all. In such cases, those variables do not follow the scope rules we just
learned about.

If you declare a variable with **`var`**, that variable does not get block
scoped.

## Practice Identifying Scope

## Conclusion

<!-- See if you can determine which variables are globally scoped:

```js
const name = "Kentaro";

function greeting(name, type) {
  const hi = "Hello there, ";
  const bye = "See you next time, ";

  if (type === "hi") {
    console.log(hi + name);
  } else {
    console.log(bye + name);
  }
}

greeting(name, "bye");
```

<details><summary><b>Which variables from the above snippet are in the global scope?</b></summary>
<p>
  <ul>
    <li>name</li>
    <li>greeting</li>
  </ul>
</p>
</details>

<details><summary><b>Bonus Question: What would the output be of the above example?</b></summary>
<p>
  See you next time, Kentaro
</p>
</details> -->
