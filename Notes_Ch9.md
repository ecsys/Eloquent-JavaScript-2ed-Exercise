# Chapter 9 Regular Expression

## Creating a Regular Expression

    - construct with RegExp constructor
        - `var re1 = new RegExp("abc");`
    - construct with literal value
        - `var re2 = /abc/`
    - greedy vs non-greedy
        - greedy operator(+, ?, *, {})  
            - `"1 /* a */ + /* b */ 1".replace(/\/\*[^]*\*\//,"");`
            - -> 1  1
        - non-greedy operator(+?, ??, *?, {}?)
            - `"1 /* a */ + /* b */ 1".replace(/\/\*[^]*?\*\//,"");`
            - -> 1  +  1 
            - repetition operators matches as short as possible
    - 

## Regex Object
    
    - test
        - `/abc/.test("abcde")` -> true
        - returns true if there is a matching
    - exec
        - `var match = /\d+/.exec("one two 100");`
            - match == ["100"]
            - match.index == 8
        - returns an object with information of the first match if there is one
            - if the regex contains subexpressions grouped with parentheses
                - first element in the array is the whole match
                - next element is the part matched by the first grouped subexpression
                - next element is the second group
                - `/bad(ly)?/.exec("bad") -> ["bad", undefined]
                - `/(\d)+/.exec("123")` -> ["123", "3"] -> the last match of
                  the group displays
        - returns null otherwise
        - String has match() behaves similarly
            - `"one two 100".match(/\d+/);` -> ["100"]
            - returns an array of matched strings with globle(g) option
    - lastIndex
        - lastIndex is a property of RegExp, controls where the next match will
          start
        - condition: the global(g) option need to be enabled
        - condition: lastIndex works only with exec() method
        - a call to exec() will update lastIndex
        - lastIndex will be set back to 0 if no matching found
    - search
        - similar to String.indexOf()
        - returns the first index of matching, or -1 if there's no matching
        - `"  word".search(/\S/);` -> 2
    - String.replace(RegExp, Arg2)
        - `"Borobudur".replace(/[ou]/, "a");` -> Barobudur
            - the first match is replaced
            - use a g option to replace all matches `/[ou]/g`
        - `"Last, First".replace(/([\w ]+), ([\w ]+)/, "$2 $1");` -> First Last
            - matched group can be refer by $1, $2 ... , $9
            - whole match is $&
        - Arg2 can be function

```JavaScript
var stock = "1 lemon, 2 cabbages, and 101 eggs";
function minusOne(match, amount, unit) {
  amount = Number(amount) - 1;
  if (amount == 1) // only one left, remove the 's'
    unit = unit.slice(0, unit.length - 1);
  else if (amount == 0)
    amount = "no";
  return amount + " " + unit;
}
console.log(stock.replace(/(\d+) (\w+)/g, minusOne));
// → no lemon, 1 cabbage, and 100 eggs  
```

## Cheat Sheet

|   Regex  |               Meaning                       |   RegExp Example                      |   Match Example           |
|----------|---------------------------------------------|---------------------------------------|---------------------------|
| /abc/    | A sequence of Chars                         |                                       |                           |
| /[abc]/  | Any Char from a set of Chars                |                                       |                           |
| /[^abc]/ | Any Char not in a set of Chars              | /[^]/ - meaning not empty             | anything - multiple lines |
| /[0-9]/  | Any Char in a range of Chars                |                                       |                           |
| /x+/     | 1 or more occurrences of x                  |                                       |                           |
| /x+?/    | 1 or more occurrences, non greedy           |                                       |                           |
| /x*/     | Any occurrence                              |                                       |                           |
| /x?/     | 0 or 1 occurrence                           |                                       |                           |
| /x{2,4}/ | 2 - 4 occurrences                           | /\d{1,2}-\d{1,2}-\d{4} \d{1,2}:\d{2}/ | 30-1-2004 3:23            |
| /(abc)/  | A group                                     | /boo+(hoo+)+/i                        | BooHooooHoo               |
| /a|b|c/  | Any one of several patterns                 | /\d+(a|b|c)/                          | 105c                      |
| /\d/     | Any digit Char                              |                                       |                           |
| /\w/     | Any alphanumeric Char (Latin alphabet only) |                                       |                           |
| /\s/     | Any whitespace Char                         |                                       |                           |
| /./      | Any Char except newlines                    |                                       |                           |
| /\b/     | A word boundary (not a Char)                | /\bhaha\b/                            | x haha x                  |
| /^/      | Start of input                              | /x^/                                  | match nothing             |
| /$/      | End of input                                |                                       |                           |
