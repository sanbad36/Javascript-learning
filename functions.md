# Functions()

// Default Parameters

```jsx
// Default Parameters
const bookings = [];

const createBooking = function (
  flighNum,
  numPassengers = 1,
  price = 199 * numPassengers
) {
  // ES5
  // numPassengers = numPassengers || 1;
  // price = price || 199;

  const booking = {
    flightNum,
    numPassengers,
    price,
  };
  console.log(booking);
  bookings.push(booking);
};

createBooking('LH123');
createBooking('LH123', 2, 800);
createBooking('LH123', 2);
createBooking('LH123', 5);

createBooking('LH123', undefined, 1000);
```

### ***How Passing Arguments Works: Value vs. Reference***

```jsx
const flight = 'LH234';
const jonas = {
  name: 'Jonas Schmedtmann',
  passport: 24739479284,
};

const checkIn = function (flightNum, passenger) {
  flightNum = 'LH999';
  passenger.name = 'Mr. ' + passenger.name;

  if (passenger.passport === 24739479284) {
    alert('Checked in');
  } else {
    alert('Wrong passport!');
  }
};

// checkIn(flight, jonas);
// console.log(flight);   // LH234    // premitive type
// console.log(jonas);   // Mr. Jonas Schmedtmann     // it is pointing to the same object in the memory heap

// Is the same as doing...
// const flightNum = flight;
// const passenger = jonas;

const newPassport = function (person) {
  person.passport = Math.trunc(Math.random() * 100000000000);
};

newPassport(jonas);
checkIn(flight, jonas);
```

 `JavaScript does not have the  the passing by reference, it only have the passing by value.` 

In the above example, it seems like it is passing by reference when we have passed the Object as a parameter, actually it is the address in the heap we have passed, but still it is the value in the JavaScript.

### ***First-Class and Higher-Order Functions***

Storing the functions as the variables.

passing functions as a arguments to Other functions.

Retuning the functions from a function

call method on functions

### ***Higher Order Functions***

A function that receives another function as an argument, that returns  a new function,  or both

This is possible because of the fist order functions

```jsx
const greet = () => console.log('Hey Jonas');
btncolse addEventListener('Click',greet) // greet here is the callback function 
 // and addEventListener is the Higher Order function

```

### ***Functions Accepting Callback Functions***

```jsx
const oneWord = function (str) {
  return str.replace(/ /g, '').toLowerCase();
};

const upperFirstWord = function (str) {
  const [first, ...others] = str.split(' ');
  return [first.toUpperCase(), ...others].join(' ');
};

// Higher-order function
const transformer = function (str, fn) {
  console.log(`Original string: ${str}`);
  console.log(`Transformed string: ${fn(str)}`);

  console.log(`Transformed by: ${fn.name}`);
};

transformer('JavaScript is the best!', upperFirstWord);
transformer('JavaScript is the best!', oneWord);

// JS uses callbacks all the time
const high5 = function () {
  console.log('👋');
};
document.body.addEventListener('click', high5);
['Jonas', 'Martha', 'Adam'].forEach(high5);
```

### ***Functions Returning Functions***

```jsx
const greet = function (greeting) {
  return function (name) {
    console.log(`${greeting} ${name}`);
  };
};

const greeterHey = greet('Hey');
greeterHey('Jonas');
greeterHey('Steven');

greet('Hello')('Jonas');

const greetArr = greeting => name => console.log(`${greeting} ${name}`);

greetArr('Hi')('Jonas');
```

### ***The call and apply Methods***

```jsx
const lufthansa = {
  airline: 'Lufthansa',
  iataCode: 'LH',
  bookings: [],
  // book: function() {}
  book(flightNum, name) {
    console.log(
      `${name} booked a seat on ${this.airline} flight ${this.iataCode}${flightNum}`
    );
    this.bookings.push({ flight: `${this.iataCode}${flightNum}`, name });
  },
};

lufthansa.book(239, 'Jonas Schmedtmann');
lufthansa.book(635, 'John Smith');

const eurowings = {
  airline: 'Eurowings',
  iataCode: 'EW',
  bookings: [],
};

const book = lufthansa.book; 

// Does NOT work
 // book is not there outside. So the this keywird in inside the book will point to Window object
// therefore the it will throw error.
// book(23, 'Sarah Williams');

***// Call method
// for which object it should be point to, first argument and all the argument is the same as the functions argument***
book.call(eurowings, 23, 'Sarah Williams');
console.log(eurowings);

book.call(lufthansa, 239, 'Mary Cooper');
console.log(lufthansa);
 
const swiss = {
  airline: 'Swiss Air Lines',
  iataCode: 'LX',
  bookings: [],
};

book.call(swiss, 583, 'Mary Cooper');

// Apply method
const flightData = [583, 'George Cooper'];
book.apply(swiss, flightData);
console.log(swiss);

book.call(swiss, ...flightData);
```

In JavaScript, a [function](https://www.javascripttutorial.net/javascript-function/) is an instance of the `[Function](https://www.javascripttutorial.net/javascript-function-type/)` type. For example:

```
function show(){
    //...
}

console.log(show instanceof Function); // true
Code language: Python (python)
```

The `Function` type has a method named `call()` with the following syntax:

```
functionName.call(thisArg, arg1, arg2, ...);
Code language: Python (python)
```

The `call()` method calls a function `functionName` with a given `this` value and arguments.

The first argument of the `call()` method `thisArg` is the `this` value. It allows you to set the `this` value to any given object.

The remaining arguments of the `call()` method `arg1`, `arg2`,… are the arguments of the function.

When you invoke a function, the JavaScript engine invokes the `call()` method of that function object.

Suppose that you have the `show()` function as follows:

```
function show() {
   console.log('Show function');
}
Code language: Python (python)
```

And invoke the `show()` function:

```
show();
Code language: Python (python)
```

It is equivalent to invoke the `call()` method on the `show` function object:

```
show.call();
Code language: Python (python)
```

By default, the `[this](https://www.javascripttutorial.net/javascript-this/)` value inside the function is set to the global object i.e., `window` on web browsers and `global` on node.js:

```
function show() {
    console.log(this);
}

show();
Code language: Python (python)
```

Note that in the strict mode, the `this`  inside the function is set to `undefined` instead of the global object.

See the following example:

```
function add(a, b) {
    return a + b;
}

let result = add.call(this, 10, 20);
console.log(result);
Code language: Python (python)
```

Output:

```
30
Code language: Python (python)
```

In this example, instead of calling the `add()` function directly, we use the `call()` method to invoke the `add()` function. The `this` value is set to the global object.

See the following code:

```
var greeting = 'Hi';

var messenger = {
    greeting: 'Hello'
}

function say(name) {
    console.log(this.greeting + ' ' + name);
}
Code language: Python (python)
```

Inside the `say()` function, we reference the `greeting` via the `this` value.

If you just invoke the `say()` function via the `call()` method as follows:

```
say.call(this,'John');
Code language: Python (python)
```

It will show the following result:

```
"Hi John"
Code language: Python (python)
```

However, when you invoke the `call()` method of `say()` function and pass the `messenger` object as the `this` value:

```
say.call(messenger,'John');
Code language: Python (python)
```

The output will be:

```
"Hello John"
Code language: Python (python)
```

In this case, the `this` value inside the `say()` function references the `messenger` object, not the global object.

## Using the JavaScript `call()` method to chain constructors for an object

The `call()` method can be used for chaining constructors for an object. Consider the following example:

```
function Box(height, width) {
    this.height = height;
    this.width  = width;
}

function Widget(height, width, color) {
    Box.call(this, height, width);
    this.color = color;
}

let widget = new Widget('red',100,200);
Code language: Python (python)
```

In this example:

- First, initialize the `Box` object with two properties: `height` and `width`.
- Second, invoke the `call()` method of the `Box` object inside the `Widget` object, set the `this` value to the `Widget` object.

## Using the JavaScript `call()` method for function borrowing

The following defines two objects: `car` and `aircraft`:

```
const car = {
    name: 'car',
    start: function() {
        console.log('Start the ' + this.name);
    },
    speedup: function() {
        console.log('Speed up the ' + this.name)
    },
    stop: function() {
        console.log('Stop the ' + this.name);
    }
};

const aircraft = {
    name: 'aircraft',
    fly: function(){
        console.log('Fly');
    }
};

Code language: Python (python)
```

The `aircraft` object has the `fly()` method.

The following code uses the `call()` method to invoke the `start()` method of the `car` object on the `aircraft` object:

```
car.start.call(aircraft);
Code language: Python (python)
```

Here is the output:

```
Start the aircraft
Code language: Python (python)
```

Technically, the `aircraft` object has borrowed the `start()` method of the `car` object for the `aircraft`.

When an object uses a method of another object is called the function borrowing.

The typical applications of function borrowing are to use the built-in methods of the [Array](https://www.javascripttutorial.net/javascript-array/) type.

For example, the `arguments` object inside a function is an array-like object, not an array object. To use the `[slice()](https://www.javascripttutorial.net/javascript-array-slice/)` method of the Array object, you need to use the `call()` method:

```
function getOddNumbers() {
    const args = Array.prototype.slice.call(arguments);
    return args.filter(num => num % 2);
}

let oddNumbers = getOddNumbers(10, 1, 3, 4, 8, 9);
console.log(oddNumbers);
Code language: Python (python)
```

Output:

```
[ 1, 3, 9 ]
Code language: Python (python)
```

In this example, we passed any number of numbers into the function. The function returns an array of odd numbers.

The following statement uses the `call()` function to set the `this` inside the `slice()` method to the `arguments` object and execute the `slice()` method:

```
const args = Array.prototype.slice.call(arguments);

```

### ***The bind Method***

```jsx
// book.call(eurowings, 23, 'Sarah Williams');

const bookEW = book.bind(eurowings);

const bookLH = book.bind(lufthansa);

const bookLX = book.bind(swiss);

bookEW(23, 'Steven Williams');

const bookEW23 = book.bind(eurowings, 23);

bookEW23('Jonas Schmedtmann');

bookEW23('Martha Cooper');

// With Event Listeners

lufthansa.planes = 300;

lufthansa.buyPlane = function () {

console.log(this);

this.planes++;

console.log(this.planes);

};

// lufthansa.buyPlane();

document

.querySelector('.buy')

.addEventListener('click', lufthansa.buyPlane.bind(lufthansa));

// Partial application

const addTax = (rate, value) => value + value * rate;

console.log(addTax(0.1, 200));

const addVAT = addTax.bind(null, 0.23);

// addVAT = value => value + value * 0.23;

console.log(addVAT(100));

console.log(addVAT(23));

const addTaxRate = function (rate) {

return function (value) {

return value + value * rate;

};

};

const addVAT2 = addTaxRate(0.23);

console.log(addVAT2(100));

console.log(addVAT2(23));
```

The `bind()` method returns a new [function](https://www.javascripttutorial.net/javascript-function/), when invoked, has its `[this](https://www.javascripttutorial.net/javascript-this/)` sets to a specific value.

The following illustrates the syntax of the `bind()` method:

```
fn.bind(thisArg[, arg1[, arg2[, ...]]])
Code language: CSS (css)
```

In this syntax, the `bind()` method returns a copy of the function `fn` with the specific `this` value (`thisArg`) and arguments (`arg1`, `arg2`, …).

Unlike the `[call()](https://www.javascripttutorial.net/javascript-call/)` and `[apply()](https://www.javascripttutorial.net/javascript-apply-method/)` methods, the `bind()` method doesn’t immediately execute the function. It just returns a new version of the function whose `this` sets to `thisArg` argument.

### ***JavaScript Immediately Invoked Function Expression*** IIFEs

A JavaScript immediately invoked function expression is a [function](https://www.javascripttutorial.net/javascript-function/) defined as an expression and executed immediately after creation. The following shows the syntax of defining an immediately invoked function expression:

```
(function(){
//...
})();

Code language: JavaScript (javascript)
```

## Why IIFEs

When you define a [function](https://www.javascripttutorial.net/javascript-function/), the JavaScript engine adds the function to the global object. See the following example:

```
function add(a,b) {
    return a + b;
}
Code language: JavaScript (javascript)
```

On the Web Browsers, the `add()` function is added to the `window` object:

```
console.log(window.add);
Code language: JavaScript (javascript)
```

Similarly, if you declare a [variable](https://www.javascripttutorial.net/javascript-variables/) outside of a function, the JavaScript engine also adds the variable to the global object:

```
var counter = 10;
console.log(window.counter);// 10
Code language: JavaScript (javascript)
```

If you have many global variables and functions, the JavaScript engine will only release the memory allocated for them until when the global object loses the scope.

As a result, the script may use the memory inefficiently. On top of that, having global variables and functions will likely cause the name collisions.

One way to prevent the functions and variables from polluting the global object is to use immediately invoked function expressions.

In JavaScript, you can have the following expressions:

```
'This is a string';
(10+20);
Code language: JavaScript (javascript)
```

This syntax is correct even though the expressions have no effect. A function can be also declared as an expression which is called a function expression:

```
let add = function(a, b) {
    return a + b;
}
Code language: JavaScript (javascript)
```

In this syntax, the part on the right side of the assignment operator(`=`) is a function expression. Because a function is an expression, you can wrap it inside parentheses:

```
let add = (function(a, b) {
    return a + b;
});
Code language: JavaScript (javascript)
```

In this example, the `add` variable is referenced as the [anonymous function](https://www.javascripttutorial.net/javascript-anonymous-functions/) that adds two arguments.

In addition, you can execute the function immediately after creating it:

```
let add = (function(a,b){
    return a + b;
})(10, 20);

console.log(add);
Code language: JavaScript (javascript)
```

In this example, the `add` variable holds the result of the function call.

The following expression is called an immediately invoked function expression (IIFE) because the function is created as an expression and executed immediately:

```
(function(a,b){
        return a + b;
})(10,20);
Code language: JavaScript (javascript)
```

This is the general syntax for defining an IIFE:

```
(function(){
//...
})();

Code language: JavaScript (javascript)
```

Note that you can use an [arrow function](https://www.javascripttutorial.net/es6/javascript-arrow-function/) to define an IIFE:

```
(() => {
//...
})();
Code language: JavaScript (javascript)
```

By placing functions and [variables](https://www.javascripttutorial.net/javascript-variables/) inside an immediately invoked function expression, you can avoid polluting them to the global object:

```
(function() {
    var counter = 0;

    function add(a, b) {
        return a + b;
    }

    console.log(add(10,20));// 30
}());

```

## What are callbacks

In JavaScript, a callback is a [function](https://www.javascripttutorial.net/javascript-function/) passed into another function as an argument to be executed later.

Suppose that you the following `numbers` array:

```
let numbers = [1, 2, 4, 7, 3, 5, 6];
Code language: JavaScript (javascript)
```

To find all the odd numbers in the array, you can use the `[filter()](https://www.javascripttutorial.net/javascript-array-filter/)` method of the [Array](https://www.javascripttutorial.net/javascript-array/) object.

The `filter()` method creates a new array with the elements that pass the test implemented by a function.

The following test function returns `true` if a number is an odd number:

```
function isOddNumber(number) {
    return number % 2;
}
Code language: JavaScript (javascript)
```

Now, you can pass the `isOddNumber()` to the `filter()` method:

```
const oddNumbers = numbers.filter(isOddNumber);
console.log(oddNumbers);// [ 1, 7, 3, 5 ]
Code language: JavaScript (javascript)
```

In this example, the `isOddNumber` is a callback function. When you pass a callback function into another function, you just pass the reference of the function i.e., the function name without the parentheses `()`.

To make it shorter, you can use an anonymous function as a callback:

```
let oddNumbers = numbers.filter(function(number) {
    return number % 2;
});
console.log(oddNumbers);// [ 1, 7, 3, 5 ]
Code language: JavaScript (javascript)
```

In ES6, you can use the [arrow functions](https://www.javascripttutorial.net/es6/javascript-arrow-function/):

```
let oddNumbers = numbers.filter(number => number % 2);
Code language: JavaScript (javascript)
```

When you use the JavaScript on web browsers, you often listen to an event e.g., a button click and carry some actions if the event occurs.

Suppose that you have a button with the id `btn`:

```
<button id="btn">Save</button>
Code language: HTML, XML (xml)
```

To execute some code when the button is clicked, you use a callback and pass it to the `addEventListener()` method:

```
function btnClicked() {
// do something here
}
let btn = document.querySelector('#btn');
btn.addEventListener('click',btnClicked);
Code language: JavaScript (javascript)
```

The `btnClicked` in this example is a callback. When the button is clicked, the `btnClicked()` function is c
