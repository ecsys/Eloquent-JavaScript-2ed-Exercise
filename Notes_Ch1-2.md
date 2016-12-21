# Chapter 1 - Values, Types and Operators

- Numbers
  - stores in 8 bytes (+/- 9 quadrillions)
  - calculations with fractional numbers are not precise, can be treated as approximation
  - special numbers
  - Infinity / -Infinity
  - NaN (zero / zero, Infinity - Infinity...)

- String
  - can be enclosed in both single and double quote
  - string comparison - based on UNICODE standard

- Boolean
  - NaN == NaN // false
  - null == 0 // false
  - "" == false // true
  - "" === false // false
      - === operator tests precisely euqal
  - "", 0, NaN are counted as false

- null and undefined
  - null == undefined // ture
  - null and undefined are interchangale for the most cases

- automatic type conversion
  - 8 * null   // -> 0 
  - "5" - 1    // -> 4
  - "5' + 1    // -> 51
  - "five" * 2 // -> NaN

# Chapter 2 - Program Structure

## Comment Format

```javascript
// inline comment

/*
  block comment
*/
```
