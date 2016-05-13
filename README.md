# JavaScript Variables

## Objectives
+ Declare a variable without assigning a value
+ Declare and define a variable
+ multi-line variable assignment
+ Explain how local and global variables differ


## About

As in Ruby, variables in JavaScript are used to store data that will be used in our program.  A variable can point to almost any type of value including numbers, strings, arrays, and hashes.

Just like Ruby, variables are assigned values using the `=` operator. Variable names are typically all lower case, and in the case of multiple words, the words are joined together using [lowerCamelCase](http://c2.com/cgi/wiki?LowerCamelCase).

## Declaring Variables

Lets say I have the variable `word`. In Ruby, to assign a value to this variable, we would simple do

```ruby
word = "hey"
```

Ruby would understand automatically that we're creating a new variable and assigning it a value. If this variable was created in a method, it would only exist in the scope of the method. If it was created in a block, it would only exist in the scope of that block. Later in the method or block when you used that variable, Ruby wouldn't think to look outside the block for the variable definition.

JavaScript variables operates a little differently, and scope in JavaScript operates a lot differently. JavaScript variables must be declared before they can be assigned a value. If you don't declare your variable, JavaScript will bubble up through layers of scope (up out of the function you defined your variable in), till it finds a declared variable with that name. This means you could end up using different values than you thought you were.

Go ahead and open up a Chrome or Firefox browser window, and open up the Developer Tools. Feel free to code along with these examples.

Declaring a variable without defining a value looks like this:

```js
var word;
```

Now try entering `word` in console. You should see `Undefined` because we never defined a value for this variable. Now I can assign `word` a value:
```js
word = "hey";
```

When you enter `word` you should see `hey`. This works, but feels pretty tedious. Thankfully we can declare and define a variable all on one line:

```js
var word = "hey";
```

## Multi-line Variable Assignment

Let's say I needed to declare and define multiple variables. It feels like a lot to have to repeat `var` over and over again. JavaScript allows us to do multi-line variable assignment to alleviate this pain. Every variable must be separated with a comma, and end the entire line must end with a semicolon.

Let's condense the below code into one line:

```javascript
var a = 5;
var b = 2;
var c = 3;
var d = {};
var e = [];
```

The above is equivalent to:

```javascript
var a = 5,
    b = 2,
    c = 3,
    d = {},
    e = [];
```

which can be converted to:

```javascript
var a = 5, b = 2, c = 3, d = {}, e = [];
```

Try returning each variable in the console. You should see the appropriate values returned for each one.

Some people prefer to keep new lines between each new variable assignment, other people like the look of the single line. Whichever way you swing, the important thing to remember is [to use commas to separate each variable](http://stackoverflow.com/a/4166789/2890716).

## Reassigning Variable Value

Changing the value of a variable in JavaScript works just in the same way as it does in Ruby:

```js
var word = "hey";
word;
// returns "hey"
word = "javascript";
word;
// returns "javascript";
```

## Local vs. Global Variables

Just like Ruby, JavaScript also has local and global variables. In Ruby, a program is written within the scope `Main`. The JavaScript equivalent is `window` (the browser window).

We'll dive much deeper into scope in JavaScript, but for all intents and purposes, a global variable is any variable defined within the `window`. You can also think of it as any variable that exists outside of a function (or method) is a global variable.



```javascript
var firstNum = 10; // is defined in the window and is thus accessible by the entire program
window.firstNum;
// returns 10
```

But if we declare a variable inside a function:

```js
function myFunk(){
  var funky = true;
}

myFunk();
window.funky;
// returns undefined because funky is variable defined inside a function and is thus a variable local to that function.
```
These consequences are due to JavaScript's approach to scoping. In Ruby, we didn't have to worry too much about scoping because all variables assigned within a method are scoped to just that method. For instance:

```ruby
def make_variable
  cute_animal = "sugar glider"
  return cute_animal
end

make_variable
# => "sugar glider"

cute_animal
# => NameError: undefined local variable or method `cute_animal' for main:Object
```

Unlike Ruby, JavaScript will make a variable have local scope only if you use the keyword `var`. For instance:

```javascript
function makeVariable() {
  cuteAnimal = "sugar glider";
  return cuteAnimal;
}

makeVariable();
// "sugar glider"

cuteAnimal;
// "sugar glider"
```

Now might be a good time to drop to your knees, throw your fists into the air, and scream-ask, "What?!?"

Let me give you a second. You ready? Okay, here's what's going on:

JavaScript does not treat variables as local by default; you must use the keyword `var`. Without this keyword, variables have a global scope. JavaScript knew about the variable `cuteAnimal` because we accidentally gave it a global scope. To make it local in scope (always what you want; don't pollute that global namespace with variables!), you'd have to add that `var` keyword, like so:

```javascript
function makeVariable() {
  var cuteAnimal = "sugar glider";
  return cuteAnimal;
}

makeVariable();
// "sugar glider"

cuteAnimal;
// ReferenceError: cuteAnimal is not defined
```

It's been mentioned already, but again, it is best to use the key word `var` before declaring a variable. This ensures that the variable is set to the current scope. If `var` is not used in defining a new variable it becomes global and is accessible throughout the program.

Keep in mind too that the same variable name used in different scopes is effectively a different variable.
We sometimes refer to repeating a variable name in an inner scope as "shadowing" — it's best to avoid,
as you'll quickly see how confusing it can be:

``` javascript
var cuteAnimal = 'quokka';

function makeVariable() {
  var cuteAnimal = 'sugar glider';
  return cuteAnimal;
}

makeVariable();
// 'sugar glider'

cuteAnimal;
// 'quokka'

```

See? Nothing breaks, but if you have a lot of shadowed variables (or even just a lot of space between a
variable's initial declaration and its subsequent change(s)), you're gonna have a bad time.

## Changing Variable Values

Local variable assignment can overwrite global variable assignment:

```javascript
// declares a global variable called "volume" and sets it to 10
volume = 10;

function returnEleven () {
  // declares a local variable called "volume" and sets it to 11
  var volume = 11;  
  return volume;
}

// returns 11
returnEleven();
// the global variable is still 10
volume;

function goToEleven(){
  // changes the global variable to 11
  volume = 11;  
  return volume;
}

// returns 11
goToEleven();
// the global variable `volume` has been changed to 11
volume;
```

The same principle applies in reverse. Declaring a variable in an inner scope (say, inside a function) doesn't make it available to an outer scope (say, `window` (or `global` if we're in Node.js)).

```javascript
function sayHello() {
  var greeting = "hola";
  greeting = "hello";
  return greeting;
}

// Returns "hello"
sayHello()
// Even though `sayHello()` initializes the variable `greeting` with the string
// "hola", the very next line assigns the string "hello" to the variable `
// greeting`, which is the value we get when `sayHello()` returns `greeting`.

// However, if we attempt to access the variable `greeting` in the outer scope,
// we get an error
greeting
// ReferenceError: greeting is not defined
// This demonstrates that we have only declared the variable `greeting` inside
// `sayHello()`'s scope.
```


## Resources

* [MDN - Var Keyword](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)
* [MSDN - Variable Scope](https://msdn.microsoft.com/library/bzt2dkta(v=vs.94).aspx)
* [StackOverflow - How to define multiple variables on a single line?](http://stackoverflow.com/q/4166785/2890716)

<p data-visibility='hidden'>View <a href='https://learn.co/lessons/intro-to-variables.js' title='JavaScript Variables'>JavaScript Variables</a> on Learn.co and start learning to code for free.</p>

<p data-visibility='hidden'>View <a href='https://learn.co/lessons/intro-to-variables.js'>Intro To Variables in JS</a> on Learn.co and start learning to code for free.</p>
