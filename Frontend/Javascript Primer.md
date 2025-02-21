# Objects
JavaScript’s ==objects are dictionaries that map property names to values==. You can **create an object** by calling a constructor, e.g. 
- new Object(), or 
by writing an object literal, e.g. 
```JavaScript
var daddysShirt = { 
	type: 'polo', 
	color: 'white', 
	numButtons: 3 
};
```


To **access and assign values to the properties of an object**, you use the `.` operator, e.g.,
- daddysShirt.color(Evaluates to ‘white’)
and
- daddysShirt.type = 'guayabera'; 
- daddysShirt.numButtons = 7; 
- daddysShirt.brand = 'Banana Republic';

When you assign a value to a property of an object, that property will be added automatically if the object doesn’t already have it.

And if you don’t know the name of a property at compile time, there’s also a square bracket syntax that lets you compute the name dynamically, e.g.

- `var propName = 'num' + 'Buttons';` 
- `daddysShirt[propName]; (Evaluates to 7)`

==Property names are always strings==. If you try to access or assign to a non-string property of an object, e.g.,
- `var someObject = { foo: 42, bar: 'hello world' };`
- `daddysShirt[someObject];`

the non-string value will be implicitly converted to a string via its `toString()` method.

Last but not least, you can **remove a property** using the `delete` statement. For example, after, 
- `delete daddysShirt.color;`

the expression `daddysShirt.color`will evaluate to `undefined`.
# Delegation
JavaScript is a _prototype-based_ language: ==every object has a prototype from which it may inherit properties==. For example, the prototype of all strings is `String.prototype`, which is where methods like `indexOf` and `substr` are defined.

When people say that an object _delegates to_ its prototype, they’re talking about the **property look-up** operation. 

Here’s how the _prototype chain_ is used for property look-up: if you look up the value of a property `p` in `obj` and `obj` doesn’t have it, the JavaScript interpreter will automatically look it up in `obj`’s prototype. If `obj`’s prototype doesn’t have a value for `p` either, the search will continue in `obj`’s prototype’s prototype, and so on. If we eventually reach `null`, which is not an object and therefore does not have any properties, then `obj.p` will evaluate to the special value `undefined`.

![[Pasted image 20250202162450.png]]

You can use a function called `Object.create` to **create an object that delegates to another object**. For example:
```JavaScript
// My shirt is like daddy’s shirt…
var myShirt = Object.create(daddysShirt);
```

![[Pasted image 20250202162507.png]]

Note that `myShirt` does not have any properties of its own initially; it inherits all of its properties from `daddysShirt`. This means that if we change the value of any of `daddysShirt`’s properties, e.g., with `daddysShirt.numButtons = 8;` then `myShirt`’s value for that property will change, too.

Now let’s see what happens when we assign to one of the properties of `myShirt`:

```JavaScript
// … except it’s blue. 
myShirt.color = 'blue';
```

After the assignment above, `myShirt` gets its own `color` property whose value is `'blue'`. This property _shadows_ the property that `myShirt` previously inherited from `daddysShirt`, so future changes to `daddysShirt.color` will no longer effect the value of `myShirt.color`.

![[Pasted image 20250202162708.png]]

# Functions
To **declare a function**, you use the `function` keyword:
```JavaScript
function add(x, y) { 
	return x + y; 
}
```

The example above declares a function called `add` in the current lexical scope.

Functions are _first-class_ values. ==This means you can store a function in a variable, property, or an array. You can also return a function from another function or a method==. Note that you don’t have to declare a function in order to get your hands on a function value.

In JavaScript, there are two ways to write function expressions. 
One looks similar to a function declaration:
`function(x) { return x + 1; })(5)`(Evaluates to 6)

The other, known as a **fat arrow function**, is more concise:
`[1, 2, 3].map(x => x + 1)` (Evaluates to `[2, 3, 4]`)

Here’s a more elaborate example that shows that a function can reference variables from its enclosing environment:

```JavaScript
function makeCounter() { 
	var count = 0; 
	return function() { return count++; }; 
} 

var counter = makeCounter(); 

counter(); // Evaluates to `0`.
counter(); //Evaluates to `1`. 
counter(); // Evaluates to `2`.
```

In JavaScript, you can call a function with any number of arguments, no matter how many _formal parameters_ that function has in its declaration. 

A function can access the arguments that were passed to it via its formal parameters or the `arguments` object, which is, erm, not entirely unlike an array. The `arguments` can be used to write **variadic functions**, i.e., functions that take a variable number of arguments.

```JavaScript
function sum(/* a, b, c, … */) {
  var ans = 0;
  for (var idx = 0; idx < arguments.length; idx++) {
    ans += arguments[idx];
  }
  return ans;
}

sum(5, 10, -2); // Evaluates to 13
```

**Warning!** The `arguments` object is array-like, but it’s not really an array. This is unfortunate for a number of reasons, not least of all because it means that `arguments` doesn’t support useful `Array` methods like [`map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) and [`reduce`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce). If you really want to use those methods, here’s a way to create an array that contains the arguments that were passed to your function:

`var args = Array.prototype.slice.call(arguments)`;

It’s kludgy, but it does the trick. More on the `slice` and `call` methods later.

# Scoping
JavaScript has [_lexical scoping_](https://en.wikipedia.org/wiki/Scope_\(computer_science\)#Lexical_scope_vs._dynamic_scope), but it doesn’t work the way you’d expect. You see, in most languages that are lexically-scoped, a _block statement_ (`{` … `}`) is a lexical scope. In JavaScript, the only thing that is a lexical scope is a function. 

> That’s not completely true, `catch` blocks also get their own lexical scope.

**So when you write this**:
```JavaScript
 function f(x) {

  if (x > 5) {
    var y = x * x;
    …
  }
  …
}
```
**What it really means is this:**
```JavaScript
function f(x) {
  var y;
  if (x > 5) {
    y = x * x;
    …
  }
  …
}
```

Usually this isn’t a problem, but it’s something you should be aware of. And if you’re used to shadowing variable declarations, get ready to spend countless hours debugging your programs.

# Method
A method is just a function that’s stored in a property. 
For example, if we have:

```JavaScript
var aPoint = { 
	x: -2, 
	y: 5, 
	toString: function() { 
		return '(' + this.x + ',' + this.y + ')'; 
    } 
};
```

`aPoint.toString()` will evaluate to `'(-2,5)'`.

**Warning!** 
When you call a function the usual way, e.g. `f(1, 2)`(As opposed to a method call, e.g., `o.m(1, 2)`), `this` gets bound to `undefined`. 

So if you declare a helper function inside of a method — which seems like a reasonable thing to do! — it won’t work as you would expect:

```JavaScript
({
  m1: function() {
    // here, `this` is the receiver
    this.m2();  // works

    function helper() {
      // but here, `this` is `undefined`
      this.m2();  // ERROR!
    }
    helper();
  },
  m2: function() { … }
}).m1();
```

This is a subtle bug, and JavaScript programmers get bit by it all the time. The usual work-around, once you realize that this is the problem, is to declare a local variable called `self` through which the helper function can access the receiver:

```JavaScript
({
  m1: function() {
    var self = this;
    function helper() {
      // here, `this` is still `undefined`
      // but we have access to the receiver through `self`
      self.m2();  // works!
    }
    helper();
  },
  m2: function() { … }
}).m1();
```

Another solution is to make `helper` a **fat arrow function**. Here’s what that looks like:

```JavaScript
({
  m1: function() {
    var helper = () => this.m2();  // works!
    helper();
  },
  m2: function() { … }
}).m1();
```

This works because a fat arrow function captures not only the local variables, but also `this` from its enclosing lexical scope.

# Classes
JavaScript does not have support for classes, but it does have a _syntactic sugar_ for declaring them. 

Under the hood, it’s still all just objects, delegation, etc. but the sugar really makes it feel like you’re programming in a “classy” language. 

Here’s an example of a class declaration:

```JavaScript
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ',' + this.y + ')';
  }
}
```

And here’s its _desugaring_, i.e., what it really means:

```JavaScript
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function() {
  return '(' + this.x + ',' + this.y + ')';
};
```

To understand how this works, you first have to know what happens when you call a function like a constructor, e.g., `new Point(1, 2)`:

- The JavaScript interpreter creates an object that delegates to the function’s `prototype` property, e.g., `Object.create(Point.prototype)`. Note that when you write a function, its `prototype` property automatically gets initialized to a fresh object, i.e., the result of evaluating `new Object()`.
- Didactic license strikes again! It’s not really a fresh object, but rather a new object with a single property, `constructor`, whose value is the function itself.
- The value of the `new` expression will be that new object, _unless the function returns an object_. (In which case, that object will be the value of the `new` expression.)


![[Pasted image 20250202184538.png]]

Note that all `Point`s delegate to `Point.prototype`, which makes it the ideal place to store whatever properties they share, e.g., methods like `toString`. You can see how this is done in the desugaring above.

## Inheritance, super-sends, etc.
Here’s what it looks like when you declare a subclass:

```JavaScript
class Point3D extends Point {
  constructor(x, y, z) {
    super(x, y);
    this.z = z;
  }

  // Override
  toString() {
    return '(' + this.x + ',' + this.y + ',' + this.z + ')';
  }
}
```

And here’s (roughly) its desugaring:

```Javascript
function Point3D(x, y, z) {
  // Call Point's constructor with `this` bound to the new Point3D.
  Point.call(this, x, y);

  // Finish initializing the new instance.
  this.z = z;
}

// Make Point3D's prototype delegate to Point's prototype, that
// way it will inherits all of the methods of the superclass.
Point3D.prototype = Object.create(Point.prototype);

Point3D.prototype.toString = function() {
  return '(' + this.x + ',' + this.y + ',' + this.z + ')';
};

```

The relationships among the `Point` and `Point3D` “classes”, their prototypes, and their instances are illustrated below.

![[Pasted image 20250202185027.png]]

# References
1. https://www.cs.cmu.edu/~aldrich/courses/17-396/js/
2. 