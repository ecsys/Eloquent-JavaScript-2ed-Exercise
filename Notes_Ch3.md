# Chapter 3 - Functions

- Keyword return
  - return is not optional
  - return without an expression after it will cause the function to return undefined

- Scope
  - functions are the only thing to create a new scope
  - let keyword creates a varible that is local to the enclosing block

- Optional arguments
  - if you pass too many arguments, the extra ones are ignored
  - if you pass too few arguments, the missing ones are filled with undefined

- Closure
  - A function that "closes over" some local variables is called a closure

```javascript
function wrapValue(n){
  var localVariable = n;
  return function() { return localVariable; };
}

var wrap1 = wrapValue(1);
var wrap2 = wrapValue(2);
console.log(wrap1()); // -> 1
console.log(wrap2()); // -> 2
```

```javascript
function multiplier(factor) {
  return function(number) {
    return number * factor;
  };
}

var twice = multiplier(2);
console.log(twice(5)); // -> 10
```

- Recursive

```javascript
function fundSolution(target) {
  function find(start, history) {
    if (start == target)
      return history;
    else if (start > target)
      return null;
    else
      return find(start + 5, "(" + history + " + 5)") ||
             find(start * 3, "(" + history + " * 3)");
  }
  return find(1, "1");
}
```
