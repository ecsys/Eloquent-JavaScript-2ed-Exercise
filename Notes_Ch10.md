# Chapter 10 - Modules


## Using Functions As Namespaces
    
  - functions are the only things in JavaScript that create a new scope

```JavaScript
var dayName = function() {
    var names = ["Sunday", "Monday", "Tuesday", "Wednesday", 
                 "Thursday", "Friday", "Saturday"];
    return function(number) {
        return names[number];
    };
}();

console.log(3); // -> Wednesday
```

  - *name* is a local variable in an (unnamed) function
  - The returned function is stored in *dayName*
  - we can have hundreds of local variables within the unnamed function

```JavaScript
(function() {
    function square(x) { return x * x; }
    var hundred = 100;

    console.log(square(hundred));
})();

// -> 10000
``` 
  - the above code simply print 10000 to console, but it could be a module
    that adds a method to some prototype
  - the code inside the function is isolated entirely from the outside world

## Objects as Interfaces

```JavaScript
(function(exports) {
    var names = ["Sunday", "Monday", "Tuesday", "Wednesday", 
                 "Thursday", "Friday", "Saturday"];
    exports.name = function(number) P
        return names[number];
    };
    exports.number = function(name) {
        return names.indexOf(name);
    };
})(this.weekDay = {}); // outside of a function, *this* refer to the global
scope

console.log(weekDay.name(6)); // -> Saturday
```
  - the function in parentheses is called by the parentheses after it
  - it passed an empty object {}, which is refer by weekDay, to the function
  - then we can use *weekDay.name* to refer to the exposed functions

## Evaluating Data as Code
  - eval()
      - execute a sting of code in the current scope
      - may break some properties that the current scope has
  - Function() constructor
      - `var plusOne = new Function("n", "return n+1;");`
      - two arguments:
          - a string containing a comma-separated list of argument names
          - a string containing the function's body

# Require
```JavaScript
function require(name) {
    if (name in require.cache)
        return require.cache[name];

    var code = new Function("exports, module", readFile(name));
    var exports = {};
    var module = {exports: exports};
    code(exports, module);

    require.cache[name] = module.exports;
    return module.exports;
}
require.cache = Object.create(null);
console.log(require("weekDay").name(1)); //-> Monday

// weekDay module file
var names = ["Sunday", "Monday", "Tuesday", "Wednesday",
             "Thursday", "Friday", "Saturday"];

exports.name = function(number) {
    return names[number];
};
exports.number = function(name) {
    return names.indexOf(name);
};
```
  - module file only contains the code
  - one module will only be loaded once because of the cache
  - argument *module* in the object is not used in the example
      - it can be modified in the module code in order to return something
        other than exports
  - new module system introduced in Node.js is called *CommonJS modules*
    
# Slow-Loading Modules(Asynchronous Module Definition)

```JavaScript
define(["weekDay", "today"], function(weekDay, today) {
    console.log(weekDay.name(today.dayNumber());
});
```
  - AMD allows the page to continue working while the files/modules are being
    fetched
    - AMD loads dependencies in the background
    - wrap the module in the function and calls the function when all dependencies are loaded
    - *define* is central to this approach
      - takes first an array of module names
      - and then a function that takes one argument for each denpendency 
      - *define* returns whatever was returned by the function passed to *define*
  - inplementation of *define* use object to describe to state of modules
    - we assume the loaded file contains a single call to *define*

```Javascript
// the loaded dependency calls define() once here
define([], function() {
    var names = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday",
                 "Friday", "Saturday"];
    return {
        name: function(number) { return names[number]; },
        number: function(name) { return names.indexOf(name); }
    };  
});

var defineCache = Object.create(null);
var currentMod = null;

function getModule(name) {
    if (name in defineCache) 
        return defineCache[name];
    var module = {exports: null,
                  loaded: false,
                  onLoad: []};
    defineCache[name] = module;
    // assume we have this function to read from a file
    backgroundReadFile(name, function(code) {
        currentMod = module;
        new Function("", code)(); // note this is an execution call
    });
    return module;
}

function define(depNames, moduleFunction) {
    // first time myMod is null, when define function
    // is called by each loaded module, then myMod is
    // the caller module
    var myMod = currentMod; 
    var deps = depNames.map(getModule);

    deps.forEach(function(mod) {
        if(!mod.loaded)
            mod.onLoad.push(whenDepsLoaded);
    });

    function whenDepsLoaded() {
        if (!deps.every(function(m) {return m.loaded;} ))
            return;
        
        var args = deps.map(function(m) {return m.exports; });
        var exports = moduleFunciton.apply(null, args);
        if(myMod) {
            myMod.exports = exports;
            myMod.loaded = true;
            myMod.onLoad.forEach(function(f) {f();});
        }
    }   
    whenDepsLoaded();
}
```
  - logic flow of the *define* function
    - define function is called to load a module with some dependencies
      - `define(["A", "B"], function(A, B) { here is the content of the module });`
      - *myMod* is null
      - call *getModule* on dependencies *A* and *B*
        - *getModule* create a object for module *A*
        - *getModule* set *currentMod* to module *A*
        - *getModule* execute the content of module *A*, which includes
          a call to *define*
            - `define ([], function() { content of the module A });`
            - in this *define* call, *myMod* is set to module *A*
            - directly call *whenDepsLoaded* since there is no dependencies
            - set *myMod.exports* to whatever returns from the content of module *A*
        - return the object of module *A*
        - do the samething for module *B*
      - call *whenDepsLoaded* at the end of function *define*
        - `moduleFunction.apply(null, args)` calls the content of the module with its dependencies
    - end


