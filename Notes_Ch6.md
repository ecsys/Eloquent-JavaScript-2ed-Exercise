# Chapter 6 - The Secret Life of Objects

- methods are properties that hold function values (so that such property can be called)
  - *this* variable in the body of a method points to the object it was called on
  - the first argument of `apply()` and `bind()` are used to give a value to *this*
  - the `call()` function is similar to `apply()`, but taking normal arguments rather than an array

- Object's prototype
  - `console.log(Object.getPrototypeOf({}) == Object.prototype)` -> true
  - `console.log(Object.getPrototypeOf(Object.prototype))` -> null
  - `Object.prototype` is the root prototype
    - it provides a few properties
      - `toString` converts an object to a string representation
  - `console.log(Object.getPrototypeOf(isNaN) == Function.prototype);`
  - `console.log(Object.getPrototypeOf([]) == Array.prototype);`
    - Function.prototype and Array.prototype will have Object.prototype as their prototype
  - use `Object.create(proto)` to create an object with a specific prototype
  - properties inherited from prototype can be overridden
  - *enumerable* and *nonenumerable* properties
    - *nonenumerable* properties will not be considered in `for (var name in object)` loop
    - `Object.defineProperty(object, 'property name', {enumerable: false, value: property value});`
  - Object.hasOwnProperty() will not show inherited property from prototype
    - `obj.hasOwnProperty("toString"));` -> false
    - what if `hasOwnProperty` property has been overridden? Then this function can not be accessed
      - `Object.create(null)` will return a object has *null* as prototype which has no property at all
- Constructor
  - calling a function with a *new* in front of it makes it is treates as a constructor
  - a constructor returns a new object if not explicitly specified returning something else, *this* is bound to that new object
  - constructors' names are starting with a capitalized letter
  - constructors(functions) has a property named *prototype*
    - every instance created with a constructor will have `consctructor.property` as their prototype

```javascript
function Rabbit(type) {
  this.type = type;
}
var killerRabbit = new Rabbit("killer");
Rabbit.prototype.speak = function(line){
  console.log("The " + this.type + " rabbit says " + line);
}
killerRabbit.speak("Doom.."); //-> The killer rabbit says Doom..
```
- Polymorphism
  - having different objects expose the same interface and writing codes works on any object with such interface is called polymorphism
- getter and setter

```javascript
var pile = {
  elements: ["eggshell", "orange peel", "worm"],
  get height() {
    return this.elements.length;
  },
  set height(value) {
    console.log("Ignoring attempt to set height to", value);
  }
};

console.log(pile.height);
// -> 3
pile.height = 100;
// -> Ignoring attempt to set height to 100
```
    - if only getter method but no setter method is defind, then all statement assigning value to that property is ignored
    - such a property can be added to an existing object by using the `Object.defineProperty` function

```javascript
Object.defineProperty(Textcell.prototype, "heightProp", {
  get: function() { return this.text.length; }
});
```
- Inheritance
```javascript
function RTextCell(text) {       // constructor function
  TextCell.call(this, text);     // in the constructor, it calls another constructor and pass itself as this
}                                // when calls RTextCell constructor, it calls the TextCell constructor
RTextCell.prototype = Object.create(TextCell.prototype);      // now RTextCell and TextCell have same properties
RTextCell.prototype.draw = function(){};                      // RTextCell override the draw method

console.log(new RTextCell("A") instanceof RTextCell);   // -> true
console.log(new RTextCell("A") instanceof TextCell);    // -> true
console.log(new TextCell("A") instanceof RTextCell);    // -> false
console.log([1] instanceof Array);                      // -> true
```
