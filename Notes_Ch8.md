# Chapter 8 Bugs and Error Handling

  - Strict Mode
    - `use strict` can be added at the top of a file or a function body
    - under the strict mode
      - variable cannot be declared automatically as global variable, need to use `var variable;` for declareations
      - `this` binding holds `undefined` as default in functions that are not methods
        - when it's not strict, `this` refers to the global scope object as default
  - Exception Handling
    - `Error` constructor is the standard JavaScript to create an exception
      - `throw new Error("error message goes here");` 
      - Each Error object has a *message* property
      - Each Error object has a *stack* property
    - Keyword `throw` is used to raise an exception
    - `try - catch - finally` is used to handle an exception
        - finally block always run, even if there is a `return` statement
          before it
    - Invalid use of the language will also cause exceptions, they can be
      caught like your own exceptions
        - looking up a property on `null`
        - referencing a nonexistent variable
        - calling something not a function
    - Selective catching
        - the `catch` block will catch all types of error
        - define a new type of Error
```JavaScript
function InputError(message) {
    this.message = message;
    this.stack = (new Error()).stack;
}
InputError.prototype = Object.create(Error.prototype);
InputError.prototype.name = "InputError";
```
            - `instanceof Error` will return true for `InputError`
            - has a *name* property (standard error types also have this e.g.
              SyntaxError)  
        - catch -> check whether we got if the one we are interested in ->
          re-throw it otherwise
```JavaScript
function promptDirection(question) {
    var result = prompt(question, "");
    if (result.toLowerCase() == "left") return "L";
    if (result.toLowerCase() == "right") return "R";
    throw new InputError("Invalid direction: " + result);
}

for(;;) {
    try {
        var dir = promptDirection("Where");
        console.log("Your chose ", dir);
        break;
    } catch(e) {
        if (e instanceof InputError)
            console.log("Not valid. Try again.");
        else
            throw e;
    }
}
```
  - Assertions
    - assertions are a way to make sure mistakes cause failures at the point of
      mistake
```javaScript
function AssertionFailed(message) {
    this.message = message;
}
AssertionFailed.prototype = Object.create(Error.prototype);

function assert(test, message) {
    if (!test)
        throw new AssertionFailed(message);
}

function lastElement(array) {
    // if assertion fails, it throws an error
    assert(array.length > 0, "empty array in lastElement");
    return array[array.length - 1];
}
```


