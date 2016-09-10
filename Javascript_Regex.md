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

## Matching a wild card character

A period in Regex will match any character except a new line, so it can include letters, numbers, symbols, and so on. 

```JavaScript
var str = "123 1b3 1 3 133 321";
str.match(/1.3/g); // [ '123', '1b3', '1 3', '133' ]
```

## Matching digits

```JavaScript
var str = "123 1b3 1 3 133 321";
str.match(/1\d3/g); // [ '123', '133' ]
```

## Matching alphanumeric chars

` \w ` is a word character. It will match the underscore character, numbers, or any of the 26 letters of the alphabet (in both lowercase as well as uppercase letters).

``` JavaScript
var str = "123 1b3 1 3 133 321";
str.match(/1\w3/g); // [ '123', '1b3', '133' ]
```

## Negating alphanumeric chars and digits

` \d ` will match any number, but ` \D ` will match anything except a number, since they are compliments and the same goes for ` \w ` and ` \W `.

## Defining ranges in Regex

```JavaScript
var str = "bicycles";
str.match(/[abc]/g); // [ 'b', 'c', 'c' ]
```

Regex allows you to use the ` -  dash character ` to specify a set of characters without needing to list them out. 

```JavaScript
var str = "Hello world hello World";
str.match(/[A-Z][a-z][a-z][a-z][a-z]/g); // [ 'Hello', 'World' ]
```

## Matching the dash character

```JavaScript
var str = "hello world hello-world";
str.match(/hello[\- ]world/g); // [ 'hello world', 'hello-world' ]
str.match(/hello[- ]world/g); // [ 'hello world', 'hello-world' ]
```

## Defining negated ranges

To create a negated range, you can start the range with a (^) caret character to match any character.
For example: ` /[^a-e]/ `

## Defining multipliers in Regex

- +: This matches one or more occurrences
- ?: This matches zero or one occurrence
- *: This matches zero or more occurrences


## Defining custom quantifiers

If you want to match a given character a concrete number of times, you can simply specify the number of allowed repetitions inside curly braces. This doesn't make your patterns more flexible, but it will make them shorter to read.

For example, ` /\d{3}-\d{4}/ `


## Matching n or more occurrences

If you just want to set a minimum number of times that the pattern can appear, but don't really care about the actual length, you can just add a comma after the number.

For example, ` /\w{6,}/ `


## Matching n to m occurrences

You can do this by simply adding another number after the comma.

For example, /\w{15,140}/


## Matching alternated options

```JavaScript
var str = "yes ys noo no ny yesn";
str.match(/yes|no/g); // [ 'yes', 'no', 'no', 'yes' ]
```


## Matching the beginning and end of an input

Using the (^) caret character to match the start of a string and the ($) dollar sign to match the end, we can force a pattern to be positioned in these locations

For example: ` /^word|word$/g `


## Matching word boundaries

Word boundaries are very similar to the string boundaries we just saw, except that they work in the context of a single word. For example, we want to match can, but this refers to can alone, and not can from candy. If you just type a pattern, such as ` /can/g `, you will get matches for can even if it's a part of another word, for example, in a situation where the user typed candy. Using a backslash ` \b ` character, we can denote a word boundary (either in the beginning or at the end), so that we can fix this problem using a pattern similar to ` /\bcan\b/g `.

For example: ` /\bcan\b/g ` 


## Matching nonword boundaries

Paired with the ` \b ` character, we have the ` \B ` symbol, which is its inverse. The uppercase version will put a constraint on the pattern that limits it from being at the edge of word.

```JavaScript
var str = "can candy";
str.match(/can\B/g); // [ 'can' ]  => this is the substring in "candy"
```

## Matching a whitespace character

You can match a whitespace character using the backslash s character, and it matches things such as spaces and tabs. It is similar to a word boundary, but it does have some distinctions. First of all, a word boundary matches the end of a word even if it is the last word in a pattern, unlike the whitespace character, which would require an extra space. So, ` /foo\b/ ` would match foo. However, ` /foo\s/ ` would not, because there is no following space character at the end of the string. Another difference is that a boundary matcher will count something similar to a period or dash as an actual boundary, though the whitespace character will only match a string if there is a whitespace

```JavaScript
var str = "foo foo.abc foo-a";
str.match(/foo\s/g) // [ 'foo ' ]
```

## Defining nongreedy quantifiers

In the previous section, we had a look at multipliers, where you can specify that a pattern should be repeated a certain number of times. By default, JavaScript will try and match the largest number of characters possible, which means that it will be a greedy match. Let's say we have a pattern similar to ` /\d{1,4}/ ` that will match any text and has between one and four numbers. By default, if we use 124582948, it will return 1245, as it will take the maximum number of options (greedy approach). However, if we want, we can add the ` ? ` question mark operator to tell JavaScript not to use greedy matching and instead return the minimum number of characters as possible.

```JavaScript
// do not use flag g
var str = "1234567890";
str.match(/\d{1,4}?/); // [ '1', index: 0, input: '1234567890' ]

var str01 = 'class="container" id="identifier"';
str.match(/class=".*"/); // [ 'class="container" id="identifier"', index: 0, input: 'class="container" id="identifier"' ]
str.match(/class=".*?"/); // [ 'class="container"', index: 0, input: 'class="container" id="identifier"' ]
```


## Capture groups

```JavaScript
var xml = [
			"<title>File.js</title>",
			"<size>36 KB</size>",
			"<language>JavaScript</language>",
			"<modified>5 Minutes</name>"
		  ].join("\n");

var data = {};
xml.split("\n").forEach(function(line){
  var match = /<(\w+)>([^<]*)<\/\1>/.exec(line);
  if (match) {
     var tag = match[1];
     data[tag] = match[2];
  }
});
console.log(data); // { title: 'File.js', size: '36 KB', language: 'JavaScript' }
```

` \n ` : 
Where n is a positive integer, a back reference to the last substring matching the n parenthetical in the regular expression (counting left parentheses). For example, ` /apple(,)\sorange\1/ ` matches ` 'apple, orange,' ` in ` "apple, orange, cherry, peach." ` 


## Matching non capture groups

A non capture group groups a part of a pattern but it does not actually extract this data into the results array, or use it in back referencing. One benefit of this is that it allows you to use character modifiers on full sections in your pattern. For example, if we want to get a pattern that repeats world indefinitely, we can write it as this:
` /(?:world)*/ `
This will match world as well as worldworldworld and so on. The syntax for a noncapture group is similar to a standard group, except that you start it with a question mark and a (?:) colon.

If we are parsing log messages, we may want to extract the log level and the message. The log level can be one of only a few options (such as debug, info, error, and so on), but the message will always be there. Now, you can write a pattern instead of this one:

``` JavaScript
/[info] - .*|[debug] - .*|[error] - .*/
```

We can extract the common part into its own noncapture group:

```JavaScript
/[(?:info|debug|error)] - .*/
```

By doing this we remove a lot of the duplicate code.


## Matching lookahead groups

These groups allow us to set a constraint on a pattern, but not really include this constraint in an actual match. With noncapture groups, JavaScript will not create a special index for a section, although, it will include it in the full results (the result's first element). With lookahead groups, we want to be able to make sure there is or isn't some text after our match, but we don't want this text in the results.

The group with the ` ?= ` character will mean that we want it to have this text at the end of our pattern, but we don't actually want to include it

For example, let's say we have some input text and we want to parse out all .com domain names. We might not necessarily want .com in the match, just the actual domain name. In this case, we can create this pattern:

```JavaScript
var str = "http://www.google.com http://www.baidu.com";
str.match(/\w+(?=\.com)/g); // [ 'google', 'baidu' ]
```

## Using a negative lookahead

If we wanted to use a negative lookahead, as in a lookahead group that makes sure that the included text does not follow a pattern, we can simply use an exclamation point instead of an equal to sign.

```JavaScript
// This will match all the words that do not end in a period, 
// that is, it will pull out the names from str
var str = "Mr. Smith & Mrs. Doe";
str.match(/\w+(?!\.)\b/g); // [ 'Smith', 'Doe' ]
```


# Good References
- [Regular Expressions - Javascript | MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Guide/Regular_Expressions)
































