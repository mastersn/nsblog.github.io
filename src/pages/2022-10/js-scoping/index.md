---
setup: import Layout from '/src/layouts/BlogPost.astro'
title: "All 4 JavaScript Scopes Explained"
date: "2022-10-10"
description: "There are 4 different scopes in JavaScript which each behave differently and you need to understand those differences to truly master JavaScript."
tags: ['JavaScript']
---

If you have written even a single line of JavaScript code then you have used one of the four scopes of JavaScript without even realizing it. These different scope levels determine how your code will run, how easy your code is to write/change, and many other factors about your code so knowing all the nuances of each different scope is crucial. In this short article I will teach you what each scope level is, how they interact with your code, and what you can do with this knowledge to write better cleaner code.

*If you prefer to learn visually, check out the video version of this article.*
`youtube: TkFN6e9ZDMw`

## What Is Scope?

The first question we need to tackle is what scope even is. In JavaScript, and pretty much every other programming language, your code runs within some set scope. This scope determines what variables your code has access to, how new variables will interact with the rest of your code, and a few other things. The best way to think of scope is as a partition to help you separate different parts of your code from one another.
```js
const outer = "Out"

function test() {
  const inner = "In"
  console.log(outer, inner)
  // Prints: "Out", "In"
}

test()
console.log(outer, inner)
// Throws Uncaught Reference Error: inner is not defined
```
In the above example we have some simple code that defines a variable outside a function and inside a function. We then log both variables to the console from inside and outside the function. This works fine inside the function, but outside the function our log throws an error since we do not have access to the inner variable outside the scope fo the function.

This example utilizes two of the four scopes in JavaScript to separate our code in a way that only certain parts of the code have access to certain variables. This is the main reason for different scope levels to exist, so let's cover those different scopes now.

## Scope Levels

The four different scope levels are:
1. Global Scope
2. Module Scope
3. Block Scope
4. Function Scope

This may seem like a lot to keep track of but in reality you will probably use module and block scope for 95% of all the code you write, so it is a bit easier to keep track of. This doesn't mean you should ignore the other scopes, though, as it is important to understand how they work.

### Global Scope

To get started we will talk about the simplest scope to understand which is the scope most developers use for all their code when getting started as it kind of just ignores all the restrictions the other scopes impose. To understand global scope imagine I have the following HTML/JS.
```html
<script src="script.js"></script>
```
```js
// script.js

const a = 1
console.log(a)
// Prints: 1
```
This simple example is using global scope to define its variables. The way global scope works is essentially any time you define a a variable at the top level of a file (outside any function/curly braces) it is considered global scope and can be accessed **ANYWHERE** in your entire application. This global access makes writing code easier at first since you don't have to worry about variables being blocked by different scopes, but as you start to write more complex code this quickly becomes difficult to manage. This problem is even worse when you have multiple files.
```html
<script src="script-1.js"></script>
<script src="script-2.js"></script>
```
```js
// script-1.js

const a = 1
```
```js
// script-2.js

console.log(a)
// Prints: 1
```
As you can see in the above example we defined a variable in `script-1.js` and we are able to use that variable in `script-2.js`. This is because the variable from `script-1.js` is defined globally and can be used **ANYWHERE** in your code. Because of this particular feature of global scope I try to never use global scope in any of my applications. It is incredibly difficult to keep track of global scope as your application grows which is why I much prefer module scope.

### Module Scope

Module scope is very similar to global scope, but with one minor difference. Minor scope variables are only available within the file you define them in. This means they cannot be used in other files which is ideal when trying to mentally keep track of everything. In order to enter module scope you need to use `type="module"` on your script tags. *This does much more than just change the scope so if you are unfamiliar with ES modules you should check out my [full ES module guide](/2021-11/es6-modules).*
```html
<script src="script-1.js" type="module"></script>
<script src="script-2.js" type="module"></script>
```
```js
// script-1.js

const a = 1
console.log(a)
// Prints: 1
```
```js
// script-2.js

console.log(a)
// Throws Uncaught Reference Error: a is not defined
```
With this one change we removed the biggest problem with global scope of variables being available in every file while keeping the benefits of being able to use the variable anywhere in the file it is defined. Because of this module scope is one of the two scopes I use all the time.

### Block Scope

The other major scope I use all the time is block scope. Block scope is one of the easier scopes to understand because it lines up with the curly brace `{}`. Essentially, anytime you have code inside curly braces that is considered its own block scope. This means things like functions, if statements, for loops, etc. all create their own block scope.
```js
function test() {
  const funcVar = "Func"

  if (true) {
    const ifVar = "If"
    console.log(funcVar, ifVar)
    // Prints: "Func", "If"
  }

  console.log(funVar, ifVar)
  // Throws Uncaught Reference Error: ifVar is not defined
}
```
This example has two separate block scopes being defined. Each block scope is defined as the code between the curly braces and each block has access to all the variables that are between the curly braces but **NOT** within another set of curly braces as well as all variables of its parent scope.

In our case the two blocks we have are the `test` function and the `if` statement. Our `test` scope has access to all the variables within the `test` function that are not nested in another set of curly braces which means it has access to the `funcVar` variable. Our `test` scope would also have access to any global/module scope level variables since those are parents of the `test` scope.

Our second scope is the `if` statement and that scope has access to the `ifVar` as well as everything the `test` scope has access to since the `test` scope is the parent scope of the `if` scope.

The important takeaway with block scope is anytime you have curly braces you create a new block scope that has access to all scopes it is inside of, but none of the scopes inside it. This inside/outside rule is actually true for all scopes. Any scope will have access to the scopes that it is inside, but it will have no access to the scopes inside it.
```js
{
  const a = 10
}

console.log(a)
// Throws Uncaught Reference Error: a is not defined
```
You can even add curly braces anywhere you want in your code to create a scope. This is not something I do often but it can be useful.

### Function Scope

The final scope is function scope and this is something you hopefully never have to worry about as it relates to the `var` keyword. Variables defined with the `var` keyword are scoped at the function level instead of the block level which means they only care about the curly braces of a function.
```js
function test() {
  var funcVar = "Func"

  if (true) {
    var ifVar = "If"
    console.log(funcVar, ifVar)
    // Prints: "Func", "If"
  }

  console.log(funVar, ifVar)
  // Prints: "Func", "If"
}
```
This code is exactly the same as the code from the block level example, but we are using `var` instead of `const` to define our variables. You will see in this example our code will work just fine and that is because the `var` keyword ignores block level scope so even though `ifVar` is defined within our `if` block it doesn't matter for function scope.

This is honestly pretty unintuitive and `var` as a keyword in general is difficult to use which is why I recommend never using `var`. *If you want to learn more about why I don't like `var` and the differences between `var`, `let`, and `const` check out my [complete var vs let vs const guide](/2020-01/var-vs-let-vs-const).*

## Multiple Variables With The Same Name

One important thing to understand about this scoping is how it works when you have multiple variables with the same name.
```js
function test() {
  const a = "Func"

  if (true) {
    const a = "If"
    console.log(a)
    // Prints: "If"
  }

  console.log(a)
  // Prints: "Func"
}
```
In this example we have the variable `a` defined both inside our `test` function and inside our `if` block. When we log out `a` in the `if` block we get the value of `a` from the `if` scope while when we log `a` outside the `if` block we get the value of `a` from the `test` scope.

The main takeaway from this code is that when you have two variables with the same name that are in different scopes they are actually two entirely separate variables. They have nothing to do with one another, they never overwrite each other's values and they work exactly the same as two variables with different names. The only difference is if they have the same name it is now impossible to access the value of the outer scope variable since accessing `a` will always access the most inner scope variable available.

I generally try not to use the same name of variables when writing code, but in some instances it can make writing your code easier to understand so it is sometimes necessary.

## Conclusion

While it may sound confusing to have four different scopes in JavaScript, it is actually a bit simpler than that since we really only care about two of the four scopes. Understanding how these scopes work is also crucial in writing good clean code.