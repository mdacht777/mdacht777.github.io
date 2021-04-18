---
layout: post
title:      "ES6, Arrow functions, Regular functions, and what they're useful for."
date:       2021-04-18 00:58:24 +0000
permalink:  es6_arrow_functions_regular_functions_and_what_theyre_useful_for
---

Starting in 2015 with the ECMA ES6 Standard (Google it), Javascript added a new way to declare functions.  "Arrow functions" (or "Fat arrow functions") were introduced.  This new syntax for declaring functions produced a few differences in how functions work.  In addition to being "syntactically more compact", they have some other significant contrasts to the pre-ES6 "regular functions". You can see the fine details in the MDN documentation but I have taken a few of what I consider as the key differences and tabulated them below.  There are a TON of resources and blog posts also available and while some of what I show below is based on what I was able to learn from them, I thought it would be helpful to present the information in a table for side-by-side comparison.  I hope you find this helpful.  Researching this gave me a greater understanding of the differences and, when choosing between regular functions and arrow functions, which is the best choice for the situation. 

```

```

<table style="width:100%">
  <tr>
	<td>&nbsp;</td>
	<td>Regular functions</td>
	<td>Arrow Functions </td>
  </tr>
  <tr>
	<td>Syntax</td>
	<td>
<pre>
function multiply(x, y) { 
  return x &ast; y
}
</pre>
	</td>
	<td>
<pre>
const multiply = (x, y) => { 
  return x &ast; y
}

// or on one line with a return statement (needs curly brackets) //
const multiply = (x, y) => { return x &ast; y}

// or without a return statement (no curly brackets) //
const multiply = (x, y) => x &ast; y

// or with only one argument (no parentheses around arguments) //
const multiply = x => x &ast; 3

// or no arguments - would need either () or _ //
const multiply = () => 4 &ast; 3
</pre>
	</td>
  </tr>
<tr>
<td>Argument binding</td>
<td>
Inside the body of a regular function, "arguments" is a special reserved array-like object containing the list of arguments with which the function has been invoked.
<pre>
let myFunc = {  
 showArgs(){ 
  console.log(arguments); 
 } 
}; 
myFunc.showArgs(1, 2, 3, 4); //=> Object { 0: 1, 1: 2, 2: 3, 3: 4 }
</pre>
</td>
<td>
No arguments special keyword is defined inside an arrow function.  The arguments object is resolved lexically (from the outer function).  In order to access the arguments, you have to use what are called "rest parameters".  In the example below "...args" is a "rest parameter" that holds the arguments in an array [1, 2, 3, 4].
<pre>
let myFunc = {  
  showArgs : (...args) => { 
  console.log(args); 
 } 
}; 
myFunc.showArgs(1, 2, 3, 4); //=> Array [1, 2, 3, 4]
</pre>
</td>
</tr>
<tr>
<td>Returning values</td>
<td>Regular functions require the "return" statement or an expression to evaluate.  If the return statement is missing, or there’s no expression after the return statement, the regular function implicitely returns "undefined".</td>
<td>
If the arrow function contains one expression, and you omit the function’s curly braces, then the expression is implicitly returned. There is no need for the "return" statement.
</td>
</tr>
<tr>
	<td>"this" value (aka the execution context).  **Why is this important?** Every execution context provides "this" keyword, which points to an object to which the current code that’s being executed belongs.  "this" is a way for a function to address the variables and functions it has access to.
	</td>
	<td>
	  Dynamic based on invocation:
	<ol>
	  <li>During a simple invocation the value of "this" equals to the global 
	  ("window") object (or undefined if the function runs in strict mode):<br />
<pre>
function myFunction() {
    console.log(this);
}
	  
// Simple invocation
myFunction(); // logs global object (window)
</pre>
		</li>
		<li>During a method invocation the value of "this" is the object owning the 
	  method:<br>
		<pre>
const myObject = {
  method() {
    console.log(this);
  }
};

// Method invocation
myObject.method(); // logs myObject
</pre>
</li>
	  <li>During an indirect invocation using call, apply, it is the 
	  first argument supplied:
		<br>
<pre>
function myFunction() {
  console.log(this);
}

const myContext = { value: 'A' };

myFunction.call(myContext);  // logs { value: 'A' }
myFunction.apply(myContext); // logs { value: 'A' }
</pre>
		</li>
	  <li>During a constructor invocation using the "new" keyword "this" equals to the 
	  newly created instance (see below)</li>
	</ol>
	</td>
	<td>The arrow function doesn’t define its own "this" execution context.<br />
	No matter how or where being invoked, 
	"this" value inside of an arrow 
	function always equals "this" value from the outer function - the closest "non-arrow" function:
	<pre>
const obj = {
  id: 42,
  counter: function counter() {
    setTimeout(() => {
      console.log(this.id);
    }, 1000);
  }
};

obj.counter(); //=> 42
</pre>
	</td>
  </tr>
  <tr>
  <td>Constructors</td>
  <td>Regular functions created using "function" declarations or expressions are 
  constructible and callable. They can be constructed using the "new" keyword:<br />
<pre>
function Obj (color) {
   this.color = color
}

const blueObj = new Obj('blue');
console.log(blueObj.color); // =&gt; blue
</pre>
</td>
<td>Arrow functions are anonymous (not named) so they are not constructible or callable. Using the "new" keyword throws an error:<br />
<pre>
const Obj = (color) =&gt; {
   this.color = color
}
	
const blueObj = new Obj('blue'); // =&gt; "TypeError: Obj is not a constructor".
</pre>
</td>
  </tr>
</table>

```

```

### Conclusion
There are situations when it makes more sense to use regular functions and other times when you would want to use arrow functions.  
<br>**You would want to use a regular function when you needed:**
<li>a function to be name-able and re-useable
<li>if you wanted to construct an instance of a function 
<li>if you want access to the "arguments" keyword
<li>if you want your function to be self-referencing for purposes of recursion
<li>if you want your code to be easier to debug - since arrow functions are anonymous, you will not be able to trace the name of the function or the exact line number where an error occurs.
<br>
<br>**Arrow functions are meant for short pieces of code that do not have their own “context”, but rather work in the current one.**
You would want to use arrow functions when 
<li>you just need a single-use function, for example defining a function in a callback, like setTimeout()
<li>using them with methods such as map and filter where functions don't need to be named
<li>a situation where you would need "this" to be consistent and predictable.
<li>when you want the most concise syntax for your function definition

