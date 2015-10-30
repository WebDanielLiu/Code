Advanced JavaScript Fundamentals

JavaScript is a very flexible and expressive language. It has many of the features of other dynamic languages, and a few that they don't have. Through its use of functions, closures, prototype inheritance chain, dynamic context setting and fully object orientated nature, it is also an easy language to extend.

Advanced JavaScript, by design, makes heavy use of composition, duck typing, closures and functional programming techniques. It does not have formal classes, though it's trivial to add that layer in via a small amount of code. Instead, it has a versatile and terse object literal syntax that, when combined with closures and its use of first class functions, makes it possible to utilise a more expressive array of programming patterns than available to a strict static, class based language.

Below are some advanced JavaScript fundamentals. Ranging from simple (global minimisation) through to complex (constructor functions and prototypes). In order to really leverage JavaScript, to work with the grain and in the spirit of the language, it's necessary to understand and appreciate all of the concepts presented here.

Anyone that's played with JavaScript for any amount of time, whether they realised it or not, will have applied some of these principles. For example, adding event handlers relies on passing around functions, the concepts of callbacks and most typical library examples today are built heavily around the use of closures. JavaScript libraries like jQuery and Dojo leverage the prototype chain at their core.

Digging into these techniques and understanding them will open the way for you to write more expressive, elegant and versatile JavaScript code.

Topics covered:

Global minimisation
Variable Scope & Hoisting
Dynamic / Loose Typing
Accessing Object Properties and Methods
Lexical Scope and Closures
Meaning of 'this'
Composition, Abstraction, Functional Programming and Mixins
Objects and Encapsulation
JavaScript's Object Orientated Nature
Constructor Functions and Prototypes
Global minimisation
Let's keep the global namespace clean of variables and functions.

Global namespace pollution increases the likelihood of variable name collision, silent overwriting of functionality, leading to unexpected and unreliable results. There are also some browser issues related to mapping of global variables from name properties in Internet Explorer that can be very problematic.

An application should only warrant one or two globals and in some circumstances, apps can be built with zero variables polluting the global / window space.

The two techniques to achieve this (which are typically used in combination) are as follows:

// Let's avoid doing this
var foo = "",
    bar = "";

// Instead, namespace via an object (eg. below only adds one global variable, QUX)
var QUX = {
    foo: "Apple",
    bar: "Banana"
};

QUX.foo; // "Apple"
QUX.bar; // "Banana"

// And leverage function scope
(function () {
    var foo = "",
        bar = "";
}());
The above pattern is also useful for renaming and sandboxing variables. For example, the following is commonly used with jQuery (or other libraries) to provide sandboxed access to $:

var $ = ...; // $ used by some other library

(function ($) {
    // Use $ here safely for jQuery, regardless of what was $ outside
    // $ == jQuery;
}(jQuery));
Refactoring tip for dealing with legacy code:

Leveraging an immediately invoked anonymous function (which provides function scope) like above, is also useful in containing variables in existing legacy code. If you have a large body of JavaScript with a lot of global variables and functions, you can wrap the entire body in an anonymous function, thereby sandboxing everything that was declared via 'var' in the global space. Then, by going through and checking any remaining variables to make sure they are declared correctly (eg. using 'var'), you can proceed to selectively expose as globals the bare minimum of core functions and variables that are called globally. This can be very effective in reducing legacy code that has hundreds of globals down to a few while you refactor the code base.

Variable Scope & Hoisting
It's important to understand how JavaScript 'sees' variables and the impact hoisting has on declarations. JavaScript has function scope, which is very powerful when combined with it's ability to use closures and functional programming techniques.

Scope and var
var foo = "hi";

function bar1() {
    return foo; // global foo
}
console.log(bar1(), foo); // "hi", "hi"

function bar2() {
    var foo = "bye"; // local foo
    return foo;
}
console.log(bar2(), foo); // "bye", "hi"

function bar3() {
    foo = "yay"; // global foo
    return foo;
}
console.log(bar3(), foo); // "yay", "yay"

function bar4() {
    var foo; // local foo
    return foo;
}
console.log(bar4(), foo); // undefined, "yay"
Hoisting
JavaScript hoists variable declarations to the top of the current scope, which can lead to some unexpected outcomes if you're not aware of what's going on:

function bar5() {
    return foo; // local foo (due to hoisting)
    var foo = "boo";
}
console.log(bar5(), foo); // undefined, "yay"

/*
// What's happening above

function bar5() {
    var foo;
    return foo; // foo == undefined
    foo = "boo";
}
*/

function bar6() {
    var foo = foo; // local foo (undefined, due to hoisting)
    return foo;
}
console.log(bar6(), foo); // undefined, "yay"

/*
// What's happening above

function bar6() {
    var foo;
    foo = foo; // foo == undefined
    return foo;
}
*/
Dynamic / Loose Typing
JavaScript is a dynamic / loosely typed language. The lineage of an object is irrelevant. What matters about an object is what it can do, not what it is descended from.

A variable can be initialised with any value, and can be reassigned to contained any other type of value.

Reassign variable types
// Create a variable
var foo;

// Let's add a simple function into the mix for later
var bar = function () {
    return 2;
};

// And store whatever you like in it, regardless of its type

foo = 10;
console.log(typeof foo); // number

foo = "A";
console.log(typeof foo); // string

foo = true; 
console.log(typeof foo); // boolean

foo = {}; 
console.log(typeof foo); // object

foo = []; 
console.log(typeof foo, foo.constructor == Array, foo instanceof Array); // object, true, true

foo = bar; 
console.log(typeof foo); // function

foo = function () {};
console.log(typeof foo); // function

foo = /\s/g;
console.log(typeof foo, foo.constructor == RegExp, foo instanceof RegExp); // object, true, true

foo = (10 / bar()); 
console.log(typeof foo); // number

foo = null;
console.log(typeof foo, foo === null); // object, true
Checking equality
There are two versions of the equality operator in JavaScript.

== Type coerces values to the same type before comparison, then produces true if the operands are the same value

!= Type coerces values to the same type before comparison, then produces false if the operands are the same value

=== Produces true if the operands are the same value and type

!== Produces false if the operands are the same value and type

The implicit type coercion when using == or != can be useful if you are knowingly expecting it, but can be confusing if you're not aware of what's going on under the hood.

Truthy and Falsy values
JavaScript has the concept of "truthy" values and "falsy" values. Understanding this can allow for elegant shortcut evaluation.

// Truthy (values that evaluate to true)
true;
"0";
"a string";
[];
{};
1;

// Falsy (values that evaluate to false)
0;
"";
NaN;
null;
undefined;
false;
Basic equality checking differences
// Basic equality checking differences

0 == '0' // true (== performs type coercion)
0 === '0' // false (=== checks type and value)
Type coercion in equality checking
// All the following are true (due to type coercion)

1 == true // true
0 == false // true
"" == false // true
"0" == false // true
" \t\r\n " == false // true
"" == 0 // true
"0" == 0 // true
" \t\r\n " == 0 // true
null == undefined // true

// The same checks using type checking equality operator are false

0 === false // false
"" === false // false
"0" === false // false
" \t\r\n " === false // false
"" === 0 // false
"0" === 0 // false
" \t\r\n " === 0 // false
null === undefined // false

// All of the following are false

false == "false" // false
"" == "0" // false
undefined == false // false
null == false // false
Mixed Arrays
// Create an array and store mixed items of any type
var foo = [10, "A", true, {}, [], bar, function () {}, /\s/g, (10 / bar())];
Nested Objects and Arrays
Objects and arrays can be nested to any depth (this is the basis for how JSON works, with some restrictions, eg. no functions, quote all property names). For example, an array of car objects:

var cars = [{
        make: "Ford", 
        model: "Falcon", 
        service: [2001, 2003, 2004, 2007],
        specs: {cylinders: 6, wheels: 4}
    }, {
        make: "Toyota", 
        model: "Land Cruiser", 
        service: [2004, 2005, 2008],
        specs: {cylinders: 6, wheels: 4}
    }, {
        make: "Mazda", 
        model: "RX7", 
        service: [2005, 2008, 2009, 2010],
        specs: {cylinders: 6, wheels: 4}
    }
];

var car = cars[1];

car.make; // Toyota
car.specs.wheels; // 4
Accessing Object Properties and Methods
Given an object, the following shows the two standard options for accessing a property or method:

var obj = {
    foo: "Hello",
    bar: "Goodbye",
    baz: function () {
        return "World";
    },
    zag: function () {
        return "San Dimas High";
    } 
};

// Dot notation

obj.foo; // "Hello"
obj.bar; // "Goodbye"
obj.baz(); // "World"
obj.zag(); // "San Dimas High"

// String Suffix

obj["foo"]; // "Hello"
obj["bar"]; // "Goodbye"
obj["baz"](); // "World"
obj["zag"](); // "San Dimas High"
The first form, due to its compactness and clarity, is the preferred way to access items, however the second form is very useful as it allows you to dynamically assign the property or method value from an expression, or variable.

For example:

var qux = false;
obj[qux ? "foo" : "bar"]; // "Hello" (qux == true), "Goodbye" (qux == false)
obj[qux ? "baz" : "zag"](); // "World" (qux == true), "San Dimas High" (qux == false)
As long as the result of the suffix is a string value, it will be used to look up the item, this can make it very easy to build dynamic accessor logic, reducing duplicate code and enhancing readability.

Suffix based access can be any string, even non-valid identifiers:

var obj = {
    "Glass Onion": "John Lennon"
};

var song = "Yellow Submarine";
obj[song] = "Ringo";

obj["Yellow Submarine"]; // "Ringo"
obj[song]; // "Ringo"

// Contrived complex example to demonstrate the versatility of using a string suffix

obj[["glass", "onion"].map(function (str) { return str.charAt(0).toUpperCase() + str.slice(1); }).join(" ")]; // "John Lennon"
Lexical Scope and Closures
Lexical (also called Static) scoping allows the inner function to have access to variables and functions declared in the outer context.

Closure allows the returned function to retain access to, and use of, the variables that it could see in it's original scope, even after the original outer function returns.

Lexical scoping
function foo() {
    var bar = "Apple"
    function qux() {
        // the outer bar variable is visible inside the inner function
        console.log(bar);
    }
    qux();
}

foo(); // "Apple"
Closures are useful for hiding access to variables, or enabling long lived data storage / retrieval:

function foo() {
    var count = 0;
    return function () {
        count++;
        console.log(count);
    };
}

var bar = foo();
bar(); // 1
bar(); // 2
bar(); // 3

var qux = foo();
qux(); // 1
qux(); // 2
Closures can be created anywhere by utilising immediately invoked function expressions:

var foo = {
    bar: (function() {
        var count = 0;
        return function () {
            count++;
            console.log(count);
        };
    }())
};

foo.bar(); // 1
foo.bar(); // 2
Closure is an integral part of the "Module Pattern" (more on that later):

var fooMaker = function () {
    var count = 0;
    return {
        bar: function () {
            count++;
            console.log(count);
        }
    };
};

var foo = fooMaker();

foo.bar(); // 1
foo.bar(); // 2
Meaning of 'this'
Object literals, inner functions and 'this'
var obj = {
    foo: function () {
        // this == obj
        function bar() {
            // Warning: Unfortunate language flaw ahead
            // this == window
        }
    }
};

var obj = {
    foo: function () {
        // Standard workaround for 'this' inside inner functions
        // Common var names used: self, me, that
        var self = this;
        function bar() {
            // Now we can refer to obj (eg. 'this') at any depth
            // self == obj
        }
    }
};
Functions, constructors and the 'new' keyword
function foo() {
   // this == window
   this.baz = "qux"; // Equivalent to window.baz = "qux" if function invoked normally
}

foo(); // Creates global variable baz with the value "qux"

function Foo() {
    // If called directly, eg. Foo(); then this == window
    // If invoked using 'new', eg. new Foo(); then this == object
    this.baz = "qux";
}

// Warning: Unfortunate language flaw ahead
// Calling a constructor function without new will leak variables into global scope
// Constructor functions requiring new are capitalised by convention to remind you that new is required

Foo(); // Unintended behaviour: Creates global variable baz with the value "qux"

var bar = new Foo(); // Creates and returns an object to bar
bar.baz; // bar now has a property baz with the value "qux"
Redefining the meaning of 'this' using 'call' and 'apply'
call and apply allow any function to be invoked with it's internal this set to another object context. Using call and apply with functionality built off 'this' is very useful for composition based development, allowing generic / reusable functions to be created that can be utilised by any object.

The only difference between call and apply is that call accepts a comma separated list of parameters to pass to the function being invoked, and apply accepts an array list of parameters to pass.

General syntax for call / apply
var obj1 = {},
    obj2 = {};

var fn = function (param1, param2, param3) {
    // this == window
};

fn(param1, param2, param3); // this == window
fn.call(obj1, param1, param2, param3); // this == obj1
fn.apply(obj2, [param1, param2, param3]); // this == obj2
Working example
function foo(qux, dax) {
    this.baz = qux + " " + dax;
    this.bar = function (name) {
        console.log(this.baz + " " + name);
    };
}

var obj1 = {},
    obj2 = {};

foo.call(obj1, "Hi", "there");
console.log(obj1.baz); // "Hi there"
obj1.bar("Bob"); // Logs: "Hi there Bob"

foo.apply(obj2, ["Hi", "there"]);
console.log(obj2.baz); // "Hi there"
obj2.bar("Jim"); // Logs: "Hi there Jim"
JavaScript libraries and 'this'
Many JavaScript DOM and client application libraries (eg. dojo, jQuery), will make use of features like call / apply to automatically map this for specific use cases to the context that makes sense. Ajax requests, event handlers and object / node list iterators are common examples where the library may explicitly set 'this' for you.

For example, in the Ajax callback handler for jQuery, 'this' defaults to the options object, or (if defined) the item explicitly set in those options via the 'context' property. In jQuery.fn.each 'this' is assigned to each DOM element currently being iterated over.

Setting the context in this way is done as a convenience to developers and for efficiency and brevity of code. It's important to understand that 'this' may be seeded for you automatically, or designed to deviate from non-default context rules via use of call / apply. In those cases, refer to the library API documentation.

Composition, Abstraction, Functional Programming and Mixins
Typical object literal example
var W = {
    qux: "apple",
    bar: function () {
        console.log(this.qux);
    }
};
W.bar(); // "apple"
Storing function references (thereby deriving context)
var X = {
    qux: "pear",
    bar: W.bar // store a reference to W.bar
};
X.bar(); // "pear"
Storing functions references (then redefining / extending functionality)
By storing references to existing functions in variables we can replace or extend the original function with new features.

For example, if we have a function called "foo", we can store that function reference in another variable "oldFoo", then replace foo with a new function declaration, while being able to maintain and call the original foo function.

function foo() {
    return "A";
}

var oldFoo = foo;

function foo() {
    return oldFoo() + "B";
}

console.log(foo()); // "AB"
Setting explicit context (via call / apply)
var Y = {
    qux: "grape"
};
W.bar.call(Y); // "grape"

var Z = {
    qux: "watermelon"
};
X.bar.apply(Z); // "watermelon"

X.bar.apply(W); // "apple"
Functional Programming
function foo(arr, fn) {
    for (var i = 0; i < arr.length; i++) {
        arr[i] = fn(arr[i]);
    }
    return arr;
}

function bar(item) {
    return item + "!";
}

foo(["a", "b", "c"], bar); // a!, b!, c!

foo(["x", "y", "z"], function (item) {
    return item + "?";
}); // x?, y?, z?
Mixins
Multiple objects can quickly and easily be mixed together in whole or in part using a concept called Mixins. JavaScript libraries like Dojo and jQuery, make heavy use of this concept and provide the expanded functionality through the dojo.mixin and jQuery.extend methods.

It is common to use object mixins for establishing default parameter objects that can be conveniently overwritten with a passed parameters object, to form the final settings object that a function, plugin or module will use.

For example, in jQuery using jQuery.extend, the following example function has some established defaults that can be used as is, or overwritten / expanded in whole or selectively.

// jQuery.extend example for mixin of settings object

function foo(options) {
    var defaults, settings;
    defaults = {
        color: "red",
        size: 20
    };
    settings = $.extend({}, defaults, options); 
    // Use the settings object in the function body
}

foo({color: "green"}); // override the default color with our options

/*
settings == {color: "green", size: 20}
*/
Performing a mixin is simply a matter of copying properties and values from one object to another, and unless you want to modify an existing object, you would provide an empty object as the mixin target so that a new object result is generated.

Note: Since objects are stored by reference, if the property value you are copying is an object, and you want it to be independent of the original, you would want to do a deep copy by traversing the sub objects and copying those as well.

Below is an example mixin function that takes a variable number of objects as arguments and generates a new result object if the first parameter is an empty object, or modifies an existing object and returns a reference to it.

// Simple mixin function example with shallow copy

var obj1, obj2, obj3;

obj1 = {
   color: "red",
   size: 20
};

obj2 = {
   color: "blue",
   sound: "RARRR"
};

obj3 = {
    subObj: {
        size: 50,
        name: "Fred"
    }
};

// Simple (shallow), variadic mixin function example
function mixin() {
    var baseObj, currentObj;
    baseObj = arguments[0];
    for (var i = 1, length = arguments.length; i < length; i++) {
        currentObj = arguments[i];
        for (var prop in currentObj) {
            if (currentObj.hasOwnProperty(prop)) {
                baseObj[prop] = currentObj[prop];
            }
        }
    }
    return baseObj;
}

var newObj = mixin({}, obj1, obj2, obj3.subObj);

/*
// Result
newObj == {
    color: "blue",
    sound: "RARRR",
    size: 50,    
    name: "fred"
}
*/
Objects and Encapsulation
In order to leverage the power of JavaScript, you need to think beyond class based inheritance chains. With access to functional programming, closures, context setting and lexical scoping, there are lots of options for making abstract, reusable, elegant code.

If you need a simple object, just create one. There's no need for creating a 'Class' or additional boilerplate. This is particularly the case if you are only ever creating one instance, similar to a singleton.

// Create a useful object

var copter = {
    type: "helicopter",
    noise: "chooga",
    fuel: 0,
    maxFuel: 100,
    maxAltitude: 100,
    refuel: function () {
        this.fuel = this.maxFuel;
    }
};

// Later on, if you need additional properties or methods, just add them

copter.altitude = 0;

copter.up = function () {
    this.altitude += 10;
};

// And if we need to change some functionality, change it

copter.up = function () {
    this.altitude += 10;
    this.fuel -= 10;
};

// Or augment it, while retaining the original functionality

(function () {
    var oldUp = copter.up;
    copter.up = function () {
        oldUp();
        console.log(this.noise);
    };
}());

// Or borrow them from other useful objects and functions

function land() {
    this.altitude = 0;
}

var util = {
    explode: function () {
        this.alititude = 0;
        console.log("KABOOOM!!!");
    },
    upgrade: function () {
        this.maxFuel = 200;
        this.maxAltitude = 200;
    }
};

copter.crash = util.explode;
copter.land = land;

// Or call / apply another useful function in our objects context

util.upgrade.call(copter);
JavaScript's Object Orientated Nature
Beyond the simple types (numbers, strings, booleans, null and undefined) everything in JavaSript is an object. The simple types also have an object constructor equivalent (eg. String(), Number() and Boolean()), but these aren't commonly used.

We can interact with these elements like any object, because that's exactly what they are, objects.

// Array
typeof []; // object
([]) instanceof Object; // true

// Object
typeof {}; // object
({}) instanceof Object; // true

// Function
typeof (function () {}); // function (and function is a child of object)
(function() {}) instanceof Object; // true

// Regex
typeof /\s/g; // object
/\s/g instanceof Object; // true

// etc
Object Example
var foo = {};

// Attach a property
foo.bar = "Hi";

// Add a method
foo.baz = function () {
    console.log(this.bar);
};

foo.bar; // "Hi"
foo.baz(); // "Hi"
Array Example
// Create an array
var foo = ["A", "B", "C"];

// Attach a property
foo.bar = "Hi";

// Add a method
foo.baz = function () {
    console.log(this[1]);
};

foo.bar; // "Hi"
foo.baz(); // "B"
Function Example
// Create a function
function foo() {}

// Attach a property
foo.bar = "Hi";

// Add a method
foo.baz = function () {
    console.log(this.bar);
};

foo.bar; // "Hi"
foo.baz(); // "Hi"
Regex Example
// Create a regex
var regex = /\s/g;

// Attach some properties
regex.i = 0;
regex.chars = ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m", "n"];

// Add a method
regex.set = function () {
    return this.chars[this.i++];
};

console.log("    -hello-  ".replace(regex, function () { return regex.set(); })); // abcd-hello-ef
console.log("  -goodbye-     ".replace(regex, function () { return regex.set(); })); // gh-goodbye-ijklm
... etc etc for all other object types in JavaScript

Constructor Functions and Prototypes
Constructor functions are a way to create new objects, much like a standard object, but with the following additional features:

prototype - Access to the prototype object, to add elements to the objects prototypal inheritance chain
constructor - Sets the constructor property to contain a reference to the original function constructor
instanceof - Ability to use instanceof to check if an object is an instance of a particular constructor function
Constructor functions are used with the 'new' prefix. By convention, all constructor functions should be capitalised as a signpost that 'new' should be used.

Basic constructor pattern
// Constructor function
function Foo() {}

// Create object from constructor
var obj = new Foo();

// Special object properties
obj.constructor; // Foo()
obj instanceof Foo; // true
typeof Foo.prototype; // object
Properties and methods attached to 'this' in the constructor function body will become properties and methods on the created object instances. These items are generated and independent / unique per instance.

// Constructor function
function Foo(color) {
    this.bar = color;
    this.baz = function () {
        return this.bar;
    };
}

// Create object from constructor
var obj1 = new Foo("Red"),
    obj2 = new Foo("Green");

obj1.bar; // "Red"
obj1.baz(); // "Red"

obj2.bar; // "Green"
obj2.baz(); // "Green""
Prototype object
All constructor functions have a prototype property that stores the prototype object. Initially, this object looks like this:

this.prototype = {constructor: this}
The prototype object can be replaced entirely with another object, or properties and methods can be selectively added. Any item that exists on the prototype object is available to all existing and new objects created from the constructor function.

// Constructor function
function Foo() {}

// Add property to the prototype
Foo.prototype.bar = "Blue";

// Add a method to the prototype
Foo.prototype.baz = function () {
    return this.bar;
};

// Create an object from the constructor
var obj1 = new Foo();

// Access properties or methods that do not exist directly on the object, but are returned from the objects prototype
obj1.bar; // "Blue"
obj1.baz(); // "Blue"

// Items can be added at any time to the prototype object, and they are immediately available to all past and future instances

Foo.prototype.qux = "Red";
obj1.qux; // "Red"

// Prototype items are available to all instances

var obj2 = new Foo();
obj2.qux; // "Red"
Prototype chain
JavaScript is a prototypal language. Each object has a pointer to a parent object that it inherits properties and methods from.

All objects are descendants from Object. And anything added to Object will be available to all past and present objects immediately (note: augmenting Object is not recommended for defensive programming reasons, as you are typically sharing your JavaScript environment with other scripts that are relying on expected / standard behaviour).

Each object can contain a single link to another object via it's prototype property. Initially this link points to the root Object.

Other object types like Array, Function, String and Number point to the Array, Function, String and Number objects which themselves then point to Object. This allows JavaScript to be easily extended through adding new features to the native types. As above, with augmenting Object, this is a feature that you should be aware of and understand, but for defensive programming it is best not to utilise the feature in a shared script environment.

// Object -> Object.prototype
Object.prototype.isPrototypeOf({}); // true

// Array -> Array.prototype -> Object.prototype
Array.prototype.isPrototypeOf([]) && Object.prototype.isPrototypeOf([]); // true
Object.prototype.isPrototypeOf(Array); // true

// Function -> Function.prototype -> Object.prototype
Function.prototype.isPrototypeOf(function (){}) && Object.prototype.isPrototypeOf(function (){}); // true
Object.prototype.isPrototypeOf(Function); // true

// Constructor object: Object (Foo Constructor) -> Foo.prototype -> Object.prototype
function Foo() {}
var obj1 = new Foo();
Foo.prototype.isPrototypeOf(obj1) && Object.prototype.isPrototypeOf(obj1); // true

// Chained constructor objects: Object (Bar Constructor) -> Bar.prototype -> Foo.prototype -> Object.prototype
function Bar() {}
Bar.prototype = new Foo();
var obj2 = new Bar();
Bar.prototype.isPrototypeOf(obj2) && Foo.prototype.isPrototypeOf(obj2) && Object.prototype.isPrototypeOf(obj2); // true
Delegation and the prototype chain in action
The prototype link is used only for retrieval. If we try to retrieve a property value from an object, and if the object lacks the property name, JavaScript attempts to retrieve the property value from the prototype object. And if that object is lacking the property, then it goes to its prototype, and so on until the process finally bottoms out with Object.prototype. If the desired property exists nowhere in the prototype chain, then the result is the undefined value. This lookup process is called delegation.

Object.prototype.qux = "A";
Foo.prototype.qux = "B";
Bar.prototype.qux = "C";
obj2.qux = "D";

console.log(obj2.qux); // "D"

delete obj2.qux;
console.log(obj2.qux); // "C"

delete Bar.prototype.qux;
console.log(obj2.qux); // "B"

delete Foo.prototype.qux;
console.log(obj2.qux); // "A"

delete Object.prototype.qux;
console.log(obj2.qux); // undefined
Extending base objects via prototype
To reiterate again, employing this is not recommended in shared script environments (eg. your average web application), but it's useful to understand the behaviour, as it's core to how JavaScript works and prototypal inheritance operates. It's also worth noting, that the prototype.js JavaScript library (not to be confused with the prototype object in JavaScript) was implemented using this approach.

For example, some programming languages like Groovy have a curry method that all functions can utilise via a 'curry' method. We can easily replicate this behaviour by enhancing Function.prototype to give all past, present and future JavaScript functions access to this feature.

Function.prototype.curry = function () {
    var slice = Array.prototype.slice,
        args = slice.call(arguments),
        self = this;
    return function () {
        self.apply(this, args.concat(slice.call(arguments)));
    };
};

// Create a simple function we want to curry
var myFn = function (arg1, arg2) {
    console.log(arg1, arg2);
};

// Call the curry method on our function storing the result (another function), in a new variable
var myFnCurry = myFn.curry("A");

// Invoke the new curried function
myFnCurry("B"); // Outputs: A, B
Note: You may be wondering above what the purpose of the slice on args is, that is because 'arguments' in JavaScript (due to an early implementation error, that unfortunately was solidified when other implementers reverse engineered JS) is not a true array, only array like. Passing arguments through the slice array function transforms arguments into a real array that can then utilise the standard array methods.

ECMAScript 5 Bind
ECMAScript 5 (which is gathering more support from modern browsers), has a "bind" method that allows us to do the same thing as curry above.

// Create a simple function we want to curry
var myFn = function (arg1, arg2) {
    console.log(arg1, arg2);
};

// Call the bind method on our function storing the result (another function), in a new variable
var myFnCurry = myFn.bind(this, "A");

// Invoke the new curried function
myFnCurry("B"); // Outputs: A, B
These techniques can be applied to other types like Array, String, Object etc.

Pass by value vs. pass by reference
It's important to understand the difference between items that are stored by value versus items that are stored by reference, as this can result in unexpected behaviour when using them as part of the prototype object.

// When a value on a primitive type is updated, it is changed for that instance only

function Foo() {}
Foo.prototype.qux = "Red";
Foo.prototype.list = [];

var obj1 = new Foo(),
    obj2 = new Foo();

obj2.qux = "Black";

obj1.qux; // "Red"
obj2.qux; // "Black"

// Warning: Store by reference gotcha ahead
// When an property that is shared by all instances (eg. on the prototype) and a store by reference value, has its value updated, it affects all instances

obj1.list.push("A");
obj1.list.push("B");
obj1.list.push("C");
obj1.list; // ["A", "B", "C"]

obj2.list.push("A");
obj2.list; // ["A", "B", "C", "A"]. You might have been hoping for ["A"] instead
Properties that need to be unique per instance and store values retrieved by reference, should be attached to each instances rather than sit on the prototype object.

// Approach 1 - Make it a per instance property

function Foo() {
    this.list = [];
}

// Approach 2 - Add it manually to each instance after creation

function Foo() {};
var obj1 = new Foo();
obj1.list = [];

// Combine approach 1 and 2 above

function Foo() {};

Foo.prototype.init = function () {
    this.list = [];
    // other init actions
};

var obj1 = new Foo(),
    obj2 = new Foo();

obj1.init();
obj2.init();

obj1.list.push("A");
obj1.list.push("B");
obj1.list.push("C");
obj1.list; // ["A", "B", "C"]

obj2.list.push("A");
obj2.list; // ["A"]
Assigning an object to the prototype
When completely overwriting the prototype object, you will lose the default constructor property:

function Foo() {}

Foo.prototype = {
    bar: "Hello"
};

var obj = new Foo();

obj.constructor; // Object (should be Foo)
obj instanceof Foo; // true
typeof Foo.prototype; // object
To correct this, we can manually add the constructor property back in to maintain standard behaviour:

function Foo() {}

Foo.prototype = {
    constructor: Foo,
    baz: "Hello"
};

var obj = new Foo();

obj.constructor; // Foo
obj instanceof Foo; // true
typeof Foo.prototype; // object
Public, private and privileged variables and methods
Combining all of the concepts above: Function scope, lexical scoping, closures, passing, storing and returning functions, there are a wide array of useful patterns that can be used for implementing program logic, information hiding / encapsulation and code design. For example public, private and privileged variables can be created using these principles.

Some of the more popular patterns have been given formal names, such as the "Module Pattern".

Typical example
Object factory patterns can easily utilise privacy by wrapping the logic in an immediately invoked function expression. Private variables and methods are declared inside the anonymous function body using var.

var obj = (function () {

    // Private
    var privateVariable = "A",
        privateMethod = function () {};

    return {
        // Public
        publicVariable: "B",
        publicMethod: function () {},
        // Privileged
        privilegedMethod: function (value) {
            privateVariable = value;
        }
    };

}());
Constructor example
Private variables and methods in constructor functions are a matter of declaring standard variables in the function body. Anything that you want to expose publicly is done so by attaching a property or method (or reference to a private one) to this.

function Foo() {

    // Private
    var privateVariable = "A",
        privateMethod = function () {};

    // Public
    this.publicVariable = "B";
    this.publicMethod = function () {};

    // Privileged
    this.privilegedMethod = function (value) {
        privateVariable = value;
    };

};
Prototype example
If we want to have private variables and methods (or store stateful functionality via a closure), we can make these available to the prototype object by wrapping the prototype assignment in an immediately invoked function expression.

(function () {

    // Private
    var privateVariable = "C",
        privateMethod = function () {};

    // Public
    Foo.prototype = {
        bar: 0,
        baz: function () {
            this.bar++;
            console.log(this.bar);
        },
        // Privileged
        zag: function (value) {
            privateVariable = value;
        }
    };

    Foo.prototype.qux = "D";

}());