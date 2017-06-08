# Chapter 5 Higher-Order Functions

- Abstracting Array Traversal

```javascript
function forEach(array, action) {
  for (var i=0; i < array.length; i++) 
    action(array[i]);
}

var sum = 0;
foreach(numbers, function(number) {
  sum += number;
});
console.log(sum) // -> print the result. Often, you don't pass predefined funtion to forEach.
```

- Higher-Order Functions
  - Functions operate on other functions, either by taking them as arguments or by returning them, are called *higher-order functions*
  - allow us to abstract over actions
  - functions create new functions

```javascript
function greaterThan(n) {
  return function(m) { return m > n; };
}
var greaterThan10 = greaterThan(10);
console.log(greaterThan10(11)); // -> true
```

  - functions change other functions

```javascript
function noisy(f) {
  return function(arg) {
    console.log("calling with", arg);
    var val = f(arg);
    console.log("called with", arg, "- got", val);
    return val;
  };
}
noisy(Boolean)(0);
// -> calling with 0
// -> called with 0 - got false
```

In this example, note only one parameter can be passed to `f(arg)`. This can be solved by pass an array of arguments using `apply` function.

```javascript
function transparentWrapping(f) {
  return function() {
    return f.apply(null, arguments); // -> the first parameter of apply function will be explained in the next chapter
  };
}
```

```javascript
function unless(test, then) {
  if(!test) then();
}

function repeat(times, body) {
  for(var i = 0; i < times; i++)
    body(i);
}

repeat(3, function(n) {
  unless(n % 2, function() {        // note 0 is false, thus even % 2 is false
    console.log(n, " is even");
  });
});
// -> 0 is even
// -> 2 is even
```
In the last example above, note the inner function lives inside the environment of the outer one can use n. The bodies of such inner functions can access the variables around them, but not the other way around.

  - standard usage on array
    - forEach()
    - filter() creates a new array containing all elements pass the filter in the original array
    - map() creates a new array with the same size as the original array, elements are one-to-one mapped
    - reduce() sometimes also called fold(), produce one element at a time

```javascript
function reduce(array, combine, start) {
  var current = start;
  for (var i = 0; i < array.length; i++)
    current = combine(current, array[i]);
  return current;
}

console.log(reduce([1,2,3,4], function(a,b) {
  return a + b;
}, 0)); 
// -> 10
```
  - bind() 
    - the bind() method, which all functions have, creates a new function that calls the original function with some of the arguments fixed
    - the first argument of the bind() method will be explained in the next chapter

```javascript
var theSet = ["a", "b", "c"];
function isInSet(set, e) {
  return set.indexOf(e) > -1;
}
console.log(array.filter(isInSet.bind(null, theSet))); // the bind() returns a function calls isInSet() with theSet as first arguments, followed by other remaining arguments
```
