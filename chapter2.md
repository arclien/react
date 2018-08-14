# Chapter2

## Arrow Functions
Arrow functions are a useful new feature of ES6. With arrow functions, you can cre‐ ate functions without using the function keyword. You also often do not have to use the return keyword. Example 2-2 shows the traditional function syntax.
```
//Old Fashion
var lordify = function(firstname) { 
    return `${firstname} of Canterbury`
}
console.log( lordify("Dale") ) // Dale of Canterbury
console.log( lordify("Daryle") ) // Daryle of Canterbury

// New Way 
var lordify = firstname => `${firstname} of Canterbury`;
```
With the arrow, we now have an entire function declaration on one line. The func tion keyword is removed. We also remove return because the arrow points to what should be returned. Another benefit is that if the function only takes one argument, we can remove the parentheses around the arguments.

More than one argument should be surrounded by parentheses:

```
// Old
var lordify = function(firstName, land) { return `${firstName} of ${land}`
}
// New
var lordify = (firstName, land) => `${firstName} of ${land}` console.log( lordify("Dale", "Maryland") ) // Dale of Maryland
console.log( lordify("Daryle", "Culpeper") ) // Daryle of Culpeper
```

We can keep this as a one-line function because there is only one statement that needs
to be returned.
More than one line needs to be surrounded with brackets:

```
// Old
var lordify = function(firstName, land) {
    if (!firstName) {
        throw new Error('A firstName is required to lordify')
    }
    if (!land) {
        throw new Error('A lord must have a land')
    }
    return `${firstName} of ${land}` 
}
// New
var _lordify = (firstName, land) => {
    if (!firstName) {
        throw new Error('A firstName is required to lordify')
    }
    if (!land) {
        throw new Error('A lord must have a land')
    }
    return `${firstName} of ${land}` 
}
console.log( lordify("Kelly", "Sonoma") ) // Kelly of Sonoma 
console.log( lordify("Dave") ) // ! JAVASCRIPT ERROR
```

These if/else statements are surrounded with brackets but still benefit from the shorter syntax of the arrow function.


Arrow functions do not block this. For example, this becomes something else in the setTimeout callback, not the tahoe object:

```
var tahoe = {
    resorts: ["Kirkwood","Squaw","Alpine","Heavenly","Northstar"],
    print: function(delay=1000) {
        setTimeout(function() { 
            console.log(this.resorts.join(","))
        }, delay);
    }
}
tahoe.print() // Cannot read property 'join' of undefined
```

This error is thrown because it’s trying to use the .join method on what this is. In this case, it’s the window object. Alternatively, we can use the arrow function syntax to protect the scope of this:

```
var tahoe = {
    resorts: ["Kirkwood","Squaw","Alpine","Heavenly","Northstar"], 
    print: function(delay=1000) {
        setTimeout(() => { 
            console.log(this.resorts.join(","))
        }, delay);
    }
}
tahoe.print() // Kirkwood, Squaw, Alpine, Heavenly, Northstar
```

This works correctly and we can .join the resorts with a comma. Be careful, though, that you’re always keeping scope in mind. Arrow functions do not block off the scope of this:

```
var tahoe = {
    resorts: ["Kirkwood","Squaw","Alpine","Heavenly","Northstar"], 
    print: (delay=1000) => {
        setTimeout(() => { 
            console.log(this.resorts.join(","))
        }, delay);
    }
}
tahoe.print() // Cannot read property resorts of undefined
```


Changing the print function to an arrow function means that this is actually the window.
To verify, let’s change the console message to evaluate whether this is the window:

```
var tahoe = {
    resorts: ["Kirkwood","Squaw","Alpine","Heavenly","Northstar"], 
    print: (delay=1000)=> {
        setTimeout(() => { 
            console.log(this === window);
        }, delay);
    }
}
tahoe.print();
```

It evaluates as true. To fix this, we can use a regular function:

```
var tahoe = {
    resorts: ["Kirkwood","Squaw","Alpine","Heavenly","Northstar"], 
    print: function(delay=1000) {
        setTimeout(() => { 
            console.log(this === window);
        }, delay);
    }
}
tahoe.print() // false
```


## Object Literal Enhancement
Object literal enhancement is the opposite of destructuring. It is the process of restruc‐ turing or putting back together. With object literal enhancement, we can grab vari‐ ables from the global scope and turn them into an object:

```
var name = "Tallac" var elevation = 9738
var funHike = {name,elevation}
console.log(funHike) // {name: "Tallac", elevation: 9738}
```

name and elevation are now keys of the funHike object.
We can also create object methods with object literal enhancement or restructuring:

```
var name = "Tallac";
var elevation = 9738;

var print = function() {
    console.log(`Mt. ${this.name} is ${this.elevation} feet tall`)
}
var funHike = {name,elevation,print}
funHike.print() // Mt. Tallac is 9738 feet tall
```


## Promises

Previously in JavaScript, there were no official classes. Types were defined by func‐ tions. We had to create a function and then define methods on the function object using the prototype:

```
function Vacation(destination, length) { 
    this.destination = destination this.length = length
}
Vacation.prototype.print = function() { 
    console.log(this.destination + " | " + this.length + " days")
}
var maui = new Vacation("Maui", 7);
maui.print(); // Maui | 7

```

If you were used to classical object orientation, this probably made you mad.


ES6 introduces class declaration, but JavaScript still works the same way. Functions are objects, and inheritance is handled through the prototype, but this syntax makes more sense if you come from classical object orientation:

```
class Vacation {
    constructor(destination, length) { 
        this.destination = destination 
        this.length = length
    }
    print() {
        console.log(`${this.destination} will take ${this.length} days.`)
    } 
}
```

Once you’ve created the class, you can create a new instance of the class using the new keyword. Then you can call the custom method on the class:


```
const trip = new Vacation("Santiago, Chile", 7); 
console.log(trip.print()); // Chile will take 7 days.
```

## ES6 Modules
A JavaScript module is a piece of reusable code that can easily be incorporated into other JavaScript files. Until recently, the only way to work with modular JavaScript was to incorporate a library that could handle importing and exporting modules. Now, with ES6, JavaScript itself supports modules.


JavaScript modules are stored in separate files, one file per module. There are two options when creating and exporting a module: you can export multiple JavaScript objects from a single module, or one JavaScript object per module.


In Example 2-6, text-helpers.js, the module and two functions are exported.

```
//Example 2-6. ./text-helpers.js

export const print(message) => log(message, new Date()) 

export const log(message, timestamp) =>
    console.log(`${timestamp.toString()}: ${message}`}

```

export can be used to export any JavaScript type that will be consumed in another module. In this example the print function and log function are being exported. Any other variables declared in text-helpers.js will be local to that module.

Sometimes you may want to export only one variable from a module. In these cases you can use export default (Example 2-7).

```
//Example 2-7. ./mt-freel.js
const freel = new Expedition("Mt. Freel", 2, ["water", "snack"])
export default freel
```

export default can be used in place of export when you wish to export only one type. Again, both export and export default can be used on any JavaScript type: primitives, objects, arrays, and functions.4
Modules can be consumed in other JavaScript files using the import statement. Mod‐ ules with multiple exports can take advantage of object destructuring. Modules that use export default are imported into a single variable:

```
import { print, log } from './text-helpers' 
import freel from './mt-freel'
print('printing a message')
log('logging a message')
freel.print()
```

You can scope module variables locally under different variable names:

```
import { print as p, log as l } from './text-helpers' 
p('printing a message')
l('logging a message')
```

ES6 modules are not yet fully supported by all browsers. Babel does support ES6
modules, so we will be using them throughout this book.


## CommonJS
CommonJS is the module pattern that is supported by all versions of Node.js.5 You can still use these modules with Babel and webpack. With CommonJS, JavaScript objects are exported using module.exports, as in Example 2-8.


```

//Example 2-8. ./txt-helpers.js
const print(message) => log(message, new Date()) 
const log(message, timestamp) =>
    console.log(`${timestamp.toString()}: ${message}`} 

module.exports = {print, log}

```

CommonJS does not support an import statement. Instead, modules are imported with the require function:

```
const { log, print } = require('./txt-helpers')
```