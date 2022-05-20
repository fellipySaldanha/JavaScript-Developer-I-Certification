# Objects

## ****Objects****

An object can be created with figure brackets `{…}` with an optional list of *properties*. A property is a “key: value” pair, where `key` is a string (also called a “property name”), and `value` can be anything.

We can imagine an object as a cabinet with signed files. Every piece of data is stored in its file by the key. It’s easy to find a file by its name or add/remove a file.

![Untitled](Objects-img/Untitled.png)

An empty object (“empty cabinet”) can be created using one of two syntaxes:

```jsx
let user = new Object(); // "object constructor" syntax
let user = {};  // "object literal" syntax
```

To remove a property, we can use the `delete` operator:

```jsx
delete user.age;
```

We can also use multiword property names, but then they must be quoted:

```jsx
let user = {
  name: "John",
  age: 30,
  "likes birds": true  // multiword property name must be quoted
};

console.log(user["likes birds"]); //true
```

For multiword properties, the dot access doesn’t work:

```jsx
// this would give a syntax error
user.likes birds = true

let user = {};

// set
user["likes birds"] = true;

// get
alert(user["likes birds"]); // true

// delete
delete user["likes birds"];

let key = "likes birds";

// same as user["likes birds"] = true;
user[key] = true;
```

### **Computed properties**

We can use square brackets in an object literal, when creating an object. That’s called *computed properties*.

```jsx
let fruit = prompt("Which fruit to buy?", "apple");

let bag = {
  [fruit]: 5, // the name of the property is taken from the variable fruit
};

alert( bag.apple ); // 5 if fruit="apple"
```

We can use more complex expressions inside square brackets:

```jsx
let fruit = 'apple';
let bag = {
  [fruit + 'Computers']: 5 // bag.appleComputers = 5
};
```

### **Property value shorthand**

In real code, we often use existing variables as values for property names.

```jsx
function makeUser(name, age) {
  return {
    name: name,
    age: age,
    // ...other properties
  };
}

let user = makeUser("John", 30);
alert(user.name); // John
```

In the example above, properties have the same names as variables. The use-case of making a property from a variable is so common, that there’s a special *property value shorthand* to make it shorter.

Instead of `name:name` we can just write `name`, like this:

```jsx
function makeUser(name, age) {
  return {
    name, // same as name: name
    age,  // same as age: age
    // ...
  };
}
```

We can use both normal properties and shorthands in the same object:

```jsx
let user = {
  name,  // same as name:name
  age: 30
};
```

### **Property names limitations**

As we already know, a variable cannot have a name equal to one of the language-reserved words like “for”, “let”, “return” etc.

But for an object property, there’s no such restriction:

```jsx
// these properties are all right
let obj = {
  for: 1,
  let: 2,
  return: 3
};

alert( obj.for + obj.let + obj.return );  // 6
```

In short, there are no limitations on property names. 

For instance, a number `0` becomes a string `"0"` when used as a property key:

```jsx
let obj = {
  0: "test" // same as "0": "test"
};

// both alerts access the same property (the number 0 is converted to string "0")
alert( obj["0"] ); // test
alert( obj[0] ); // test (same property)
```

### **Property existence test, “in” operator**

Reading a non-existing property just returns `undefined`. So we can easily test whether the property exists:

```jsx
let user = {};

alert( user.noSuchProperty === undefined ); // true means "no such property"
```

There’s also a special operator `"in"` for that.

The syntax is:

```jsx
"key" in object
```

For instance:

```jsx
let user = { name: "John", age: 30 };

alert( "age" in user ); // true, user.age exists
alert( "blabla" in user ); // false, user.blabla doesn't exist
```

Well, most of the time the comparison with `undefined` works fine. But there’s a special case when it fails, but `"in"` works correctly.

It’s when an object property exists, but stores `undefined`:

```jsx
let obj = {
  test: undefined
};

alert( obj.test ); // it's undefined, so - no such property?

alert( "test" in obj ); // true, the property does exist!
```

In the code above, the property `obj.test` technically exists. So the `in` operator works right.

### **The “for…in” loop**

For instance, let’s output all properties of `user`:

```jsx
let user = {
  name: "John",
  age: 30,
  isAdmin: true
};

for (let key in user) {
  // keys
  alert( key );  // name, age, isAdmin
  // values for the keys
  alert( user[key] ); // John, 30, true
}
```

### **Ordered like an object**

Integer properties are sorted, others appear in creation order.

```jsx
let codes = {
  "49": "Germany",
  "41": "Switzerland",
  "44": "Great Britain",
  // ..,
  "1": "USA"
};

for (let code in codes) {
  alert(code); // 1, 41, 44, 49
}
```

…On the other hand, if the keys are non-integer, then they are listed in the creation order, for instance:

```jsx
let user = {
  name: "John",
  surname: "Smith"
};
user.age = 25; // add one more

// non-integer properties are listed in the creation order
for (let prop in user) {
  alert( prop ); // name, surname, age
}
```

So, to fix the issue with the phone codes, we can “cheat” by making the codes non-integer. Adding a plus `"+"` sign before each code is enough.

```jsx
let codes = {
  "+49": "Germany",
  "+41": "Switzerland",
  "+44": "Great Britain",
  // ..,
  "+1": "USA"
};

for (let code in codes) {
  alert( +code ); // 49, 41, 44, 1
}
```

### Summary

Objects are associative arrays with several special features.

They store properties (key-value pairs), where:

- Property keys must be strings or symbols (usually strings).
- Values can be of any type.

To access a property, we can use:

- The dot notation: `obj.property`.
- Square brackets notation `obj["property"]`. Square brackets allow taking the key from a variable, like `obj[varWithKey]`.

Additional operators:

- To delete a property: `delete obj.prop`.
- To check if a property with the given key exists: `"key" in obj`.
- To iterate over an object: `for (let key in obj)` loop.

What we’ve studied in this chapter is called a “plain object”, or just `Object`.

There are many other kinds of objects in JavaScript:

- `Array` to store ordered data collections,
- `Date` to store the information about the date and time,
- `Error` to store the information about an error.
- …And so on.

## ****Object references and copying****

One of the fundamental differences of objects versus primitives is that objects are stored and copied “by reference”, whereas primitive values: strings, numbers, booleans, etc – are always copied “as a whole value”.

Here we put a copy of `message` into `phrase`:

```jsx
let message = "Hello!";
let phrase = message;
```

As a result we have two independent variables, each one storing the string `"Hello!"`.

![Untitled](Objects-img/Untitled%201.png)

**A variable assigned to an object stores not the object itself, but its “address in memory” – in other words “a reference” to it.**

```jsx
let user = {
  name: "John"
};
```

And here’s how it’s actually stored in memory:

![Untitled](Objects-img/Untitled%202.png)

The object is stored somewhere in memory (at the right of the picture), while the `user`
 variable (at the left) has a “reference” to it.

**When an object variable is copied, the reference is copied, but the object itself is not duplicated.**

```jsx
let user = { name: "John" };

let admin = user; // copy the reference
```

Now we have two variables, each storing a reference to the same object:

![Untitled](Objects-img/Untitled%203.png)

We can use either variable to access the object and modify its contents:

```jsx
let user = { name: 'John' };

let admin = user;

admin.name = 'Pete'; // changed by the "admin" reference

alert(user.name); // 'Pete', changes are seen from the "user" reference
```

### **Comparison by reference**

Two objects are equal only if they are the same object.

For instance, here `a` and `b` reference the same object, thus they are equal:

```jsx
let a = {};
let b = a; // copy the reference

alert( a == b ); // true, both variables reference the same object
alert( a === b ); // true
```

And here two independent objects are not equal, even though they look alike (both are empty):

```jsx
let a = {};
let b = {}; // two independent objects

alert( a == b ); // false
```

### **Cloning and merging, Object.assign**

we need to create a new object and replicate the structure of the existing one by iterating over its properties and copying them on the primitive level.

```jsx
let user = {
  name: "John",
  age: 30
};

let clone = {}; // the new empty object

// let's copy all user properties into it
for (let key in user) {
  clone[key] = user[key];
}

// now clone is a fully independent object with the same content
clone.name = "Pete"; // changed the data in it

alert( user.name ); // still John in the original object
```

Also we can use the method Object.assign for that.

```jsx
Object.assign(dest, [src1, src2, src3...])
```

- The first argument `dest` is a target object.
- Further arguments `src1, ..., srcN` (can be as many as needed) are source objects.
- It copies the properties of all source objects `src1, ..., srcN` into the target `dest`. In other words, properties of all arguments starting from the second are copied into the first object.
- The call returns `dest`.

```jsx
let user = { name: "John" };

let permissions1 = { canView: true };
let permissions2 = { canEdit: true };

// copies all properties from permissions1 and permissions2 into user
Object.assign(user, permissions1, permissions2);

// now user = { name: "John", canView: true, canEdit: true }
```

If the copied property name already exists, it gets overwritten:

```jsx
let user = { name: "John" };

Object.assign(user, { name: "Pete" });

alert(user.name); // now user = { name: "Pete" }
```

We also can use `Object.assign` to replace `for..in` loop for simple cloning:

```jsx
let user = {
  name: "John",
  age: 30
};

let clone = Object.assign({}, user);
```

### **Nested cloning**

```jsx
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

alert( user.sizes.height ); // 182
```

Now it’s not enough to copy `clone.sizes = user.sizes`, because the `user.sizes` is an object, it will be copied by reference. So `clone` and `user` will share the same sizes:

```jsx
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

let clone = Object.assign({}, user);

alert( user.sizes === clone.sizes ); // true, same object

// user and clone share sizes
user.sizes.width++;       // change a property from one place
alert(clone.sizes.width); // 51, see the result from the other one
```

**Const objects can be modified**

An important side effect of storing objects as references is that an object declared as `const` *can* be modified.

```jsx
const user = {
  name: "John"
};

user.name = "Pete"; // (*)

alert(user.name); // Pete
```

In other words, the `const user` gives an error only if we try to set `user=...` as a whole.

### Summary

Objects are assigned and copied by reference. In other words, a variable stores not the “object value”, but a “reference” (address in memory) for the value. So copying such a variable or passing it as a function argument copies that reference, not the object itself.

All operations via copied references (like adding/removing properties) are performed on the same single object.

To make a “real copy” (a clone) we can use `Object.assign` for the so-called “shallow copy” (nested objects are copied by reference) or a “deep cloning” function, such as [_.cloneDeep(obj)](https://lodash.com/docs#cloneDeep).

## ****Garbage collection****

The main concept of memory management in JavaScript is *reachability*.

Simply put, “reachable” values are those that are accessible or usable somehow. They are guaranteed to be stored in memory.

1. There’s a base set of inherently reachable values, that cannot be deleted for obvious reasons.
    
    For instance:
    
    - The currently executing function, its local variables and parameters.
    - Other functions on the current chain of nested calls, their local variables and parameters.
    - Global variables.
    - (there are some other, internal ones as well)
    
    These values are called *roots*.
    
2. Any other value is considered reachable if it’s reachable from a root by a reference or by a chain of references.
    
    For instance, if there’s an object in a global variable, and that object has a property referencing another object, *that* object is considered reachable. And those that it references are also reachable. Detailed examples to follow.
    

Here’s the simplest example:

```jsx
// user has a reference to the object
let user = {
  name: "John"
};
```

If the value of `user` is overwritten, the reference is lost:

```jsx
user = null;
```

![Untitled](Objects-img/Untitled%204.png)

Now John becomes unreachable. There’s no way to access it, no references to it. Garbage collector will junk the data and free the memory.

### **Two references**

Now let’s imagine we copied the reference from `user` to `admin`:

```jsx
// user has a reference to the object
let user = {
  name: "John"
};

let admin = user;
```

Now if we do the same:

```jsx
user = null;
```

…Then the object is still reachable via `admin` global variable, so it’s in memory. If we overwrite `admin` too, then it can be removed.

### **Interlinked objects**

Now a more complex example. The family:

```jsx
function marry(man, woman) {
  woman.husband = man;
  man.wife = woman;

  return {
    father: man,
    mother: woman
  }
}

let family = marry({
  name: "John"
}, {
  name: "Ann"
});
```

Function `marry` “marries” two objects by giving them references to each other and returns a new object that contains them both.

The resulting memory structure:

![Untitled](Objects-img/Untitled%205.png)

Now let’s remove two references:

```jsx
delete family.father;
delete family.mother.husband;
```

![Untitled](Objects-img/Untitled%206.png)

It’s not enough to delete only one of these two references, because all objects would still be reachable.

But if we delete both, then we can see that John has no incoming reference any more:

![Untitled](Objects-img/Untitled%207.png)

Outgoing references do not matter. Only incoming ones can make an object reachable. So, John is now unreachable and will be removed from the memory with all its data that also became unaccessible.

![Untitled](Objects-img/Untitled%208.png)

### **Unreachable island**

It is possible that the whole island of interlinked objects becomes unreachable and is removed from the memory.

```jsx
family = null;
```

The in-memory picture becomes:

![Untitled](Objects-img/Untitled%209.png)

This example demonstrates how important the concept of reachability is.

It’s obvious that John and Ann are still linked, both have incoming references. But that’s not enough.

The former `"family"` object has been unlinked from the root, there’s no reference to it any more, so the whole island becomes unreachable and will be removed.

### Summary

The main things to know:

- Garbage collection is performed automatically. We cannot force or prevent it.
- Objects are retained in memory while they are reachable.
- Being referenced is not the same as being reachable (from a root): a pack of interlinked objects can become unreachable as a whole.

Modern engines implement advanced algorithms of garbage collection.

A general book “The Garbage Collection Handbook: The Art of Automatic Memory Management” (R. Jones et al) covers some of them.

If you are familiar with low-level programming, the more detailed information about V8 garbage collector is in the article A tour of V8: Garbage Collection.

V8 blog also publishes articles about changes in memory management from time to time. Naturally, to learn the garbage collection, you’d better prepare by learning about V8 internals in general and read the blog of Vyacheslav Egorov who worked as one of V8 engineers. I’m saying: “V8”, because it is best covered with articles in the internet. For other engines, many approaches are similar, but garbage collection differs in many aspects.

In-depth knowledge of engines is good when you need low-level optimizations. It would be wise to plan that as the next step after you’re familiar with the language.

### ****Object methods, "this"****

### Method shorthand

There exists a shorter syntax for methods in an object literal:

```jsx
// these objects do the same

user = {
  sayHi: function() {
    alert("Hello");
  }
};

// method shorthand looks better, right?
user = {
  sayHi() { // same as "sayHi: function(){...}"
    alert("Hello");
  }
};
```

### **“this” in methods**

For instance, the code inside `user.sayHi()` may need the name of the `user`.

**To access the object, a method can use the `this` keyword.**

The value of `this` is the object “before dot”, the one used to call the method.

```jsx
let user = {
  name: "John",
  age: 30,

  sayHi() {
    // "this" is the "current object"
    alert(this.name);
  }

};

user.sayHi(); // John
```

Technically, it’s also possible to access the object without `this`, by referencing it via the outer variable.

…But such code is unreliable. If we decide to copy `user` to another variable, e.g. `admin = user` and overwrite `user` with something else, then it will access the wrong object.

```jsx
let user = {
  name: "John",
  age: 30,

  sayHi() {
    alert( user.name ); // leads to an error
  }

};

let admin = user;
user = null; // overwrite to make things obvious

admin.sayHi(); // TypeError: Cannot read property 'name' of null
```

If we used `this.name` instead of `user.name` inside the `alert`, then the code would work.

### **“this” is not bound**

In JavaScript, keyword `this` behaves unlike most other programming languages. It can be used in any function, even if it’s not a method of an object.

```jsx
function sayHi() {
  alert( this.name );
}
```

The value of `this`is evaluated during the run-time, depending on the context.

```jsx
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert( this.name );
}

// use the same function in two objects
user.f = sayHi;
admin.f = sayHi;

// these calls have different this
// "this" inside the function is the object "before the dot"
user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)

admin['f'](); // Admin (dot or square brackets access the method – doesn't matter)
```

**Calling without an object: `this == undefined`**

We can even call the function without an object at all:

```jsx
function sayHi() {
  alert(this);
}

sayHi(); // undefined
```

In this case `this` is `undefined` in strict mode. If we try to access `this.name`, there will be an error.

### **Arrow functions have no “this”**

```jsx
let user = {
  firstName: "Ilya",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  }
};

user.sayHi(); // Ilya
```

### Summary

- Functions that are stored in object properties are called “methods”.
- Methods allow objects to “act” like `object.doSomething()`.
- Methods can reference the object as `this`.

The value of `this` is defined at run-time.

- When a function is declared, it may use `this`, but that `this` has no value until the function is called.
- A function can be copied between objects.
- When a function is called in the “method” syntax: `object.method()`, the value of `this` during the call is `object`.

Please note that arrow functions are special: they have no `this`. When `this` is accessed inside an arrow function, it is taken from outside.

## ****Constructor, operator "new"****

Constructor functions technically are regular functions. There are two conventions though:

1. They are named with capital letter first.
2. They should be executed only with `"new"` operator.
3. They should have only one constructor

```jsx
function User(name) {
  this.name = name;
  this.isAdmin = false;
}

let user = new User("Jack");

alert(user.name); // Jack
alert(user.isAdmin); // false
```

![Untitled](Objects-img/Untitled%2010.png)

When a function is executed with `new`, it does the following steps:

1. A new empty object is created and assigned to `this`.
2. The function body executes. Usually it modifies `this`, adds new properties to it.
3. The value of `this` is returned.

**Omitting parentheses**

By the way, we can omit parentheses after `new`, if it has no arguments:

```jsx
let user = new User; // <-- no parentheses
// same as
let user = new User();
```

### **Methods in constructor**

For instance, `new User(name)` below creates an object with the given `name` and the method `sayHi`:

```jsx
function User(name) {
  this.name = name;

  this.sayHi = function() {
    alert( "My name is: " + this.name );
  };
}

let john = new User("John");

john.sayHi(); // My name is: John
```

### Summary

- Constructor functions or, briefly, constructors, are regular functions, but there’s a common agreement to name them with capital letter first.
- Constructor functions should only be called using `new`. Such a call implies a creation of empty `this` at the start and returning the populated one at the end.

We can use constructor functions to make multiple similar objects.

## ****Optional chaining '?.'****

The optional chaining `?.` syntax has three forms:

1. `obj?.prop` – returns `obj.prop` if `obj` exists, otherwise `undefined`.
2. `obj?.[prop]` – returns `obj[prop]` if `obj` exists, otherwise `undefined`.
3. `obj.method?.()` – calls `obj.method()` if `obj.method` exists, otherwise returns `undefined`.

As we can see, all of them are straightforward and simple to use. The `?.` checks the left part for `null/undefined` and allows the evaluation to proceed if it’s not so.

## ****Symbol type****

A “symbol” represents a unique identifier.

A value of this type can be created using `Symbol()`:

```jsx
let id = Symbol();
```

Upon creation, we can give symbol a description (also called a symbol name), mostly useful for debugging purposes:

```jsx
// id is a symbol with the description "id"
let id = Symbol("id");
```

Symbols are guaranteed to be unique. Even if we create many symbols with the same description, they are different values. The description is just a label that doesn’t affect anything.

```jsx
let id1 = Symbol("id");
let id2 = Symbol("id");

alert(id1 == id2); // false
```

**Symbols don’t auto-convert to a string**

For instance, this `alert` will show an error:

```jsx
let id = Symbol("id");
alert(id); // TypeError: Cannot convert a Symbol value to a string
```

If we really want to show a symbol, we need to explicitly call `.toString()`
 on it, like here:

```jsx
let id = Symbol("id");
alert(id.toString()); // Symbol(id), now it works
```

Or get `symbol.description` property to show the description only:

```jsx
let id = Symbol("id");
alert(id.description); // id
```

### **“Hidden” properties**

Symbols allow us to create “hidden” properties of an object, that no other part of code can accidentally access or overwrite.

```jsx
let user = { // belongs to another code
  name: "John"
};

let id = Symbol("id");

user[id] = 1;

alert( user[id] ); // we can access the data using the symbol as the key
```

### **Symbols in an object literal**

If we want to use a symbol in an object literal `{...}`, we need square brackets around it.

```jsx
let id = Symbol("id");

let user = {
  name: "John",
  [id]: 123 // not "id": 123
};
```

That’s because we need the value from the variable `id` as the key, not the string “id”.

### **Symbols are skipped by for…in**

Symbolic properties do not participate in `for..in` loop.

```jsx
let id = Symbol("id");
let user = {
  name: "John",
  age: 30,
  [id]: 123
};

for (let key in user) alert(key); // name, age (no symbols)

// the direct access by the symbol works
alert( "Direct: " + user[id] );
```

Object.keys(user) also ignores them. In contrast, Object.assign copies both string and symbol properties:

```jsx
let id = Symbol("id");
let user = {
  [id]: 123
};

let clone = Object.assign({}, user);

alert( clone[id] ); // 123
```

### **Global symbols**

use `Symbol.for(key)`. That call checks the global registry, and if there’s a symbol described as `key`, then returns it, otherwise creates a new symbol `Symbol(key)` and stores it in the registry by the given `key`.

```jsx
// read from the global registry
let id = Symbol.for("id"); // if the symbol did not exist, it is created

// read it again (maybe from another part of the code)
let idAgain = Symbol.for("id");

// the same symbol
alert( id === idAgain ); // true
```

### **Symbol.keyFor**

For global symbols, not only `Symbol.for(key)` returns a symbol by name, but there’s a reverse call: `Symbol.keyFor(sym)`, that does the reverse: returns a name by a global symbol.

```jsx
// get symbol by name
let sym = Symbol.for("name");
let sym2 = Symbol.for("id");

// get name by symbol
alert( Symbol.keyFor(sym) ); // name
alert( Symbol.keyFor(sym2) ); // id
```

The `Symbol.keyFor` internally uses the global symbol registry to look up the key for the symbol. So it doesn’t work for non-global symbols. If the symbol is not global, it won’t be able to find it and returns `undefined`. 

That said, any symbols have `description` property.

```jsx
let globalSymbol = Symbol.for("name");
let localSymbol = Symbol("name");

alert( Symbol.keyFor(globalSymbol) ); // name, global symbol
alert( Symbol.keyFor(localSymbol) ); // undefined, not global

alert( localSymbol.description ); // name
```

### **System symbols**

There exist many “system” symbols that JavaScript uses internally, and we can use them to fine-tune various aspects of our objects.

They are listed in the specification in the Well-known symbols table:

- `Symbol.hasInstance`
- `Symbol.isConcatSpreadable`
- `Symbol.iterator`
- `Symbol.toPrimitive`

### Summary

`Symbol` is a primitive type for unique identifiers.

Symbols are created with `Symbol()` call with an optional description (name).

Symbols are always different values, even if they have the same name. If we want same-named symbols to be equal, then we should use the global registry: `Symbol.for(key)` returns (creates if needed) a global symbol with `key` as the name. Multiple calls of `Symbol.for` with the same `key` return exactly the same symbol.

Symbols have two main use cases:

1. “Hidden” object properties.
    
    If we want to add a property into an object that “belongs” to another script or a library, we can create a symbol and use it as a property key. A symbolic property does not appear in `for..in`, so it won’t be accidentally processed together with other properties. Also it won’t be accessed directly, because another script does not have our symbol. So the property will be protected from accidental use or overwrite.
    
    So we can “covertly” hide something into objects that we need, but others should not see, using symbolic properties.
    
2. There are many system symbols used by JavaScript which are accessible as `Symbol.*`. We can use them to alter some built-in behaviors. For instance, later in the tutorial we’ll use `Symbol.iterator` for iterables, `Symbol.toPrimitive` to setup object-to-primitive conversion and so on.

Technically, symbols are not 100% hidden. There is a built-in method Object.getOwnPropertySymbols(obj) that allows us to get all symbols. Also there is a method named Reflect.ownKeys(obj) that returns *all* keys of an object including symbolic ones. So they are not really hidden. But most libraries, built-in functions and syntax constructs don’t use these methods.

## ****Object to primitive conversion****

The object-to-primitive conversion is called automatically by many built-in functions and operators that expect a primitive as a value.

There are 3 types (hints) of it:

- `"string"` (for `alert` and other operations that need a string)
- `"number"` (for maths)
- `"default"` (few operators, usually objects implement it the same way as `"number"`)

The specification describes explicitly which operator uses which hint.

The conversion algorithm is:

1. Call `obj[Symbol.toPrimitive](hint)` if the method exists,
2. Otherwise if hint is `"string"`
    - try calling `obj.toString()` or `obj.valueOf()`, whatever exists.
3. Otherwise if hint is `"number"` or `"default"`
    - try calling `obj.valueOf()` or `obj.toString()`, whatever exists.

All these methods must return a primitive to work (if defined).

In practice, it’s often enough to implement only `obj.toString()` as a “catch-all” method for string conversions that should return a “human-readable” representation of an object, for logging or debugging purposes.

### Links

[Objects](https://javascript.info/object)

[Symbol type](https://javascript.info/symbol)