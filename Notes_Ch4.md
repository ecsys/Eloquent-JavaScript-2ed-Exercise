# Chapter 4 - Data Structures: Objects And Arrays

- Array
  - elements in an array are stored in properties
  - array.length is a property of array
  - array.toUpperCase() is a property refers to a function value
  - push(), join(), pop() are properties of array

- Properties
  - e.g.
      - myString.length
      - Math.max
  - value.x feches the property of *value* named "x"
  - value[x] tries to evaluate the expression *x* and uses the result as the property name
      - property name as "2" is valid but can only be accessed by "value[2]"
  - propertis that contain functions are generally called methods of the value they belong
  - reading a property that doesn't exist will produce *undefined*
  - values of type *object* are arbitary collections of properties
      - *delete* operation removes properties. `delete anObject.left;` 
      - *in* operation checks if an object has the property. `console.log("left" in anObject)`
      - arrays are just a special kind of object

- Mutability
  - numbers, strings, Booleans are immutable. Their value cannot be changed.
  - objects are mutable, i.e. its value can be changed

```javascript
var a = {value: 10};
var b = a;
var c = {value: 10};
a == b; // -> true
a == c; // -> false
```

- The Arguments Objects
  - whenever a function is called, a variable named *arguments* is added to the environment in which the function body runs
  - you can pass as many as arguments you want(more or fewer) to a function call

```javascript
function logArray(flag) {
  if(flag) {
    for(var i = 1; i < arguments.length; i++) // note loop starts from 1
      console.log(arguments[i]);
  }
}
logArray(true, 1, 2, 3); // -> 1 2 3
```
- The Global Objects
  - the global scope namespace can be approached
  - in browser, it is window, i.e. `window.myVar = 10`, myVar is a global variable
