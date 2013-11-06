## What is % in JavaScript?

<http://stackoverflow.com/questions/11202824/what-is-in-javascript>

### Q

I saw `%` in many codes. Can you explain to me its purpose or what it does?

P.S: The `%` is ignored from Google searches, so I couldn't find it on Google.

Edit: I know the operand in math `13 % 10 = 3` but what I saw is like `return %foo`.

### A

Based on the link you provided in the comments, the `%` character appears to be used in some of the [V8 JavaScript engine](http://code.google.com/p/v8/) source code to indicate a method of the C++ runtime that will be executed while parsing the JavaScript source.

For example, the line in [string.js][stringjs]:

[stringjs]: http://code.google.com/searchframe#W9JxUuHYyMg/trunk/src/string.js&q=StringBuilderConcat%20package%3av8%5C.googlecode%5C.com&ct=rc&cd=5&sq=

    return %StringBuilderConcat(parts, len + 1, "");

When encountered by the parser, the `StringBuilderConcat` method will be executed. 
You can find a list of the runtime methods available to the V8 JavaScript files in [runtime.h][] (note, I have no experience with C++, 
so for all I know this has nothing to do with the `StringBuilderConcat` method referenced in string.js, but I think it's the same thing):

[runtime.h]: http://code.google.com/searchframe#W9JxUuHYyMg/trunk/src/runtime.h&q=StringBuilderConcat%20package%3av8%5C.googlecode%5C.com&ct=rc&cd=4&sq=

    #define RUNTIME_FUNCTION_LIST_ALWAYS_1(F) \
      /* Property access */ \
      F(GetProperty, 2, 1) \
      F(KeyedGetProperty, 2, 1) \
      /* ... */
      F(StringBuilderConcat, 3, 1) \
      /* ... */
  
As has already been stated, `return %foo` would throw a SyntaxError in JavaScript.
