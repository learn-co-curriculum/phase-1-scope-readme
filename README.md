# Introduction to Scope

## Learning Goals

- Define scope as a programming concept.
- Describe the three levels of scope in JavaScript.
- Examine the exceptions to the scope rules.

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
statement has its own block. Both the `if` and `else` statement have their own
sets of curly braces `{}`, and therefore have separate scopes. Anything defined
within the `if` statement, such as `favoriteFruit`, does not exist outside of
that block.

## Gotcha's to Be Aware Of

Now that we've covered the main three scopes, there are two gotcha's you should
be aware of moving forward. Both have to do with how you declare a variable.

All of our examples thus far have either used `const` or `let` to declare a
variable. For best practices, we recommend you always use one of the two.
However, you may encounter legacy code that uses `var` or doesn't use a keyword
at all. In such cases, those variables do not follow the scope rules we just
learned about.

## The `var` Gotcha

If you declare a variable with **`var`**, that variable **does not get block
scoped**. Let's demonstrate this with a modified version of our `if` statement
example from before:

```js
const animal = "capybara";

if (animal === "capybara") {
  var favoriteFruit = "yuzu";
  console.log("A capybara's favorite bath fruit is: ", favoriteFruit); // => A capybara's favorite bath fruit is: yuzu
}

console.log(favoriteFruit); // => yuzu
```

Because it was defined with `var`, we are able to access `favoriteFruit` outside
of the `if` statement it was defined in.

However, `var` does **get function scoped**. For example, let's wrap our `if`
statement inside a function:

```js
const animal = "capybara";

function logAnimalFruit() {
  if (animal === "capybara") {
    var favoriteFruit = "yuzu";
    console.log("A capybara's favorite bath fruit is: ", favoriteFruit); // => A capybara's favorite bath fruit is: yuzu
  }
  return favoriteFruit;
}

logAnimalFruit(); // => yuzu
console.log(favoriteFruit); // => ReferenceError: favoriteFruit is not defined
```

As we saw before, `favoriteFruit` does not get scoped to the `if` statement's
block. So, when we try to access the variable inside the `logAnimalFruit`
function that it's nested within, it is available. However, when we try to
access it _outside_ of the function like we did before, we are now unable to
access it.

In sum, `var` gets scoped to its nearest parent function. If it's not declared
within a function at all, it gets scoped globally.

Confusing, isn't it? This is all the more reason to **never use `var`** when
declaring variables. As long as you stick to declaring variables with `const`
and `let`, what happens in block stays in block.

## The Missing Keyword Gotcha

If you declare a variable with no keyword whatsoever, that variable **is always
globally scoped**. Regardless of where you define this variable, it will always
be accessible anywhere after its declaration.

For example, if we declare a variable with no keyword inside a function, it is
still available anywhere:

```js
function bankAccount() {
  username = "corgiluvr";
  secretPassword = "il0v3pupp!35";
  money = 1000000000;
  console.log("Account created for: " + username); // => "Account created for: corgiluvr"
}

bankAccount();
console.log(secretPassword); // => "il0v3pupp!35"

if ((secretPassword = "il0v3pupp!35")) {
  const withdraw = 1000000000;
  money = money - withdraw;
  console.log("Withdrew amount: " + withdraw); // => Withdrew amount: 1000000000
  console.log("Remaining balance: " + money); // => Remaining balance: 0
}
```

Oh no; our super secret password has leaked into the global scope and is
available everywhere! A bad actor exploited that and created a simple `if`
statement with our password to drain all our money - which was also available
everywhere.

Declaring global variables and functions should only be used as a last resort if
you absolutely need access to something **everywhere** in your program.

Again, it is best practice to always declare variables with either `let` or
`const`. Doing so will avoid troubling issues like these!

## Practice Identifying Scope

We've learned a lot! I'm sure you can see now why scope is a tricky beast that
can trip up even seasoned programmers. If you're not comfortable with these
concepts yet, that is OK. It takes time and practice. Let's get some of that in
now!

Use the following example to answer the questions below. Take your time reading
through the code to understand what's happening:

```js
let count = 0;

function increaseByNum(plusNum) {
  const increment = plusNum;
  count = count + increment;
}

function decreaseByNum(decrNum) {
  const decrement = decrNum;
  count = count - decrement;
}

for (let i = 0; i < 10; i++) {
  const counterText = "The count is currently: " + count;

  if (i < 5) {
    increaseByNum(2);
  } else {
    decreaseByNum(2);
  }

  console.log(counterText);
}

console.log(count);
```

<details><summary><b>Which variables are globally scoped?</b></summary><p>
  <ul>
    <li>count</li>
    <li>The increaseByNum function</li>
    <li>The decreaseByNum function</li>
  </ul>
</p></details>

<details><summary><b>Which variables are functionally scoped?</b></summary><p>
  <ul>
    <li>increment is functionally scoped to increaseByNum</li>
    <li>decrement is functionally scoped to decreaseByNum</li>
  </ul>
</p></details>

<details><summary><b>Which variables are block scoped?</b></summary><p>
  <ul>
    <li>counterText is block scoped to the for loop</li>
  </ul>
</p></details>

<details><summary><b>Bonus (code comprehension): What is the final count after the loop finishes?</b></summary><p>
  <ul>
    <li>It goes back down to 0</li>
  </ul>
</p></details>

## Conclusion

Let's summarize the three levels of scope we learned so you can refer back to
this as a cheatsheet.

- **Global Scope**
  - The outermost scope.
  - Anything defined at the top level of a file, outside of blocks and
    functions.
  - Accessible everywhere else in the code.
- **Functional Scope**
  - A nested scope, specifically the scope within a single function.
  - Anything defined inside the body of a function.
  - Cannot be accessed anywhere outside of the function.
- **Block Scope**
  - A nested scope, specifically the scope within asingle block, denoted by
    curly braces `{}`.
  - Anything defined inside the body of a block, such as `if` statements and
    `for` loops.
  - Cannot be accessed anywhere outside of the block.

As we've seen, scope is an important concept to keep in mind as you program.
Though difficult to grasp sometimes, it helps with writing clean code by keeping
variables only within the scope they are needed. Don't worry if it seems like a
nebulous concept right now, that is OK. Truly understanding will come with time.

## Resources

- MDN
  - [Scope](https://developer.mozilla.org/en-US/docs/Glossary/scope)
  - [Functions — Function scope](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions#Function_scope)
