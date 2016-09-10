# Regex in JavaScript

In JavaScript, regular expressions are implemented as their own type of object (such as the RegExp object). These objects store patterns and options and can then be used to test and manipulate strings.


## The RegExp constructor

Regular expressions can be created in two different ways in JavaScript, similar to the ones used in strings.

``` JavaScript
var rgx1 = new RegExp("hello");
var rgx2 = /hello/;
```

## Using pattern flags

Both forms of Regex constructors accept a second parameter, which is a string of flags. `Flags` are like settings or properties, which are applied on the entire expression and can therefore change the behavior of both the pattern and its methods.

1. `i` flag: ignore case
2. `m` flag: this makes JavaScript treat each line in the string as essentially the start of a new string
3. `g` flag: global => Without this flag, the RegExp object only checks whether there is a match in the string, returning on the first one that's found

```JavaScript
var rgx1 = new RegExp("hello", "gi");
var rgx2 = /hello/gi;
```

## Using the rgx.test method
```JavaScript
var rgx = /hello/;
rgx.test("hello"); // true
rgx.test("world"); // false
rgx.test("hello world"); // true
```

## Using the rgx.exec method
```javascript
var rgx = /world/;
rgx.exec("world !!"); // [ 'world', index: 0, input: 'world !!' ]
rgx.exec("hello world"); // [ 'world', index: 6, input: 'hello world' ]
rgx.exec("hello"); // null
```

## The string object and regular expressions

### Using the String.replace method
``` JavaScript
var str = "Li Li";
str.replace("Li", "Il"); // Il Li

str.replace(/Li/g, "Il"); // Il Il
```

### Using the String.search method
```JavaScript
str = "hello world";
str.search(/world/); // 6
```

### Using the String.match method
```JavaScript
var str = "apple";
str.match(/p/); // [ 'p', index: 1, input: 'apple' ]
str.match(/p/g); // [ 'p', 'p' ]
```





















