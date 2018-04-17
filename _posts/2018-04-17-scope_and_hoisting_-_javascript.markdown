---
layout: post
title:      "Scope and Hoisting - JavaScript"
date:       2018-04-17 10:39:03 -0400
permalink:  scope_and_hoisting_-_javascript
---


# What is Scope? 
Scope specifies the accessibility of variables, objects and functions in a particular part of your code. It is the context in which your variables exist, it determines from where you can access them and whether or not you can use them in that context. One of the advantages of scope that I really liked reading about was how it is mostly used as a way to provide a level of security to your code. Afterall, we dont want every single piece of information stored in a variable, function or object available to the world. A common theme in computer security is the principle of least access - Only allow access to what the user needs at that specific time. This helps track changes made, track bugs, keep your applications efficient and prevent others from tampering with the sensitive information stored in your code.

### JavaScript has two types of scope:

*  Global Scope
*  Local Scope


## Global Scope
The minute you start writting JavaScript on your application you are in the global scope.  All variables declared or initialized outside of a funciton are considered to be global. These variables can be accessed from anywhere in that application. 

Example 1:

```
// Declaring a global Variable:​
​var name = "Netty";
​
​// or ​
firstName = "Netty";
​
```

Example 2:

```
var name = 'Thom'

console.log(name); // logs 'Thom'

function logName() {
    console.log(name); // 'name' is accessible here and everywhere else
}

logName(); // logs 'Thom'
```

When a variable is assigned a value without first being declared with the var keyword, it is automatically added to the global context and it becomes a global variable:

```
function showNumber() {
	// Number is a global variable because it was not declared with the var keyword inside this function​
	number = 10;
	console.log(number)
}
​
showNumber(); // 10​
​
​// Number is in the global context, so it is available here as well​
console.log(number); // 10
```

## Local Scope (function-level-scope)
Variables declared within a function are considered to be of local scope. This means that these variables are only accessible within that specific function, or by other functions inside that function. 

```
var pet = "Dog"
​
​function showPet() {
	var pet = "Cat"  // local variable, only accessible in this showPet function​
	console.log(pet); // Cat
}
console.log(pet); // Dog: the global variable

```

Variable cannot be accessed outside of myFunction

```
var myFunction = function() {
  var name = 'Brina';
  console.log(name); // Brina
};
// Uncaught ReferenceError: name is not defined
console.log(name);
```

Variables can be accessed by functions inside the function they were declared in. 

```
var myFunction = function() {
  var name = 'Sam';
  var myOtherFunction = function() {
    console.log('My name is ' + name);
  };
  console.log(name);
  myOtherFunction(); // call function
};

// Will then log out:
// `Sam`
// `My name is Sam`
```


## What is Hoisting?
Every language executes from top to bottom, but JavaScript is unique in that it loads everything into memory before executing. Hoisting is a JavaScript mechanism where variables and function declarations are moved to the top of their scope before code execution. During the compiling phase variable and function declarations are put into memory, storing their specific information so that it becomes available at the top thieir scope. 

Example of calling a function that has been hoisted, saved into memory, therefore I can call it before the actual function declaration, because the infoat the top of its scope. 

```
myName("Annette");

function myName(name) {
  console.log("My first name is " + name);
}
/*
The result of the code above is: "My first name is Annette"
*/
```

Here are a few links to read more about hoisting. 
* https://scotch.io/tutorials/understanding-hoisting-in-javascript
* https://developer.mozilla.org/en-US/docs/Glossary/Hoisting
* https://www.sitepoint.com/back-to-basics-javascript-hoisting


