[Why does [1,2] + [3,4] = “1,23,4” in JavaScript?][1]

[1]: http://stackoverflow.com/questions/7124884/why-does-1-2-3-4-1-23-4-in-javascript

## question

I wanted to add the elements of an array into another, so I tried this simple sentence in our beloved Firebug:

```javascript
[1,2] + [3,4]
```

It responded with:

```javascript
"1,23,4"
```

What is going on?

## answer

The `+` operator **is not defined for arrays**.

What happens is that Javascript **converts arrays into strings** and concatenates those.

**Update**

Since this question and consequently my answer is getting a lot of attention I felt that in addition to the insightful stuff posted by [Jeremy Banks][2] it would be useful to have an **overview** about how the `+` operator behaves in general.

[2]: http://stackoverflow.com/questions/7124884/why-does-1-2-3-4-1-23-4-in-javascript/7124918#7124918

So, here it goes.

Excluding E4X and implementation-specific stuff, JavaScript has **6** built-in [data types][3]:

[3]: https://developer.mozilla.org/en/JavaScript/Reference/Operators/Special/typeof

1. undefined
2. boolean
3. number
4. string
5. function
6. object

Note that **neither** `null` **nor** `[]` is a separate type - both return `object` when fed to `typeof`. 
However `+` works differently in either case.

That's right - JavaScript has **no primitive arrays** as such; 
only instances of a class called `Array` with some syntactic sugar to ease the pain.

Adding more to the confusion, wrapper entities such as `new Number(5)`, `new Boolean(true)` and `new String("abc")` are all of object type, not numbers, booleans or strings as one might expect. 
Nevertheless for arithmetic operators `Number` and `Boolean` behave as numbers.

Easy, huh? With all that out of the way, we can move on to the overview itself.

**Different result types of + by operand types**

-------------------------------------------------------------------------------------------
            | undefined | boolean | number | string | function | object | null   | array  | 
-------------------------------------------------------------------------------------------

undefined   | number    | number  | number | string | string   | string | number | string | 

boolean     | number    | number  | number | string | string   | string | number | string | 

number      | number    | number  | number | string | string   | string | number | string | 

string      | string    | string  | string | string | string   | string | string | string | 

function    | string    | string  | string | string | string   | string | string | string | 

object      | string    | string  | string | string | string   | string | string | string | 

null        | number    | number  | number | string | string   | string | number | string | 

array       | string    | string  | string | string | string   | string | string | string | 

-------------------------------------------------------------------------------------------
\* this applies to Chrome 13, Firefox 6, Opera 11 and IE9. Checking other browsers and versions is left as an exercise for the reader.

**Note**: As pointed out by [CMS](http://stackoverflow.com/users/5445/cms), for certain cases of objects such as `Number`, `Boolean` and custom ones the `+` operator doesn't necessarily produce a string result. 
It can vary depending on the implementation of object to primitive conversion. For example `var o = { valueOf:function () { return 4; } };` evaluating `o + 2; ` produces `6`, a number, evaluating `o + '2'` produces `'42'`, a `string`.

To see how the overview table was generated visit <http://jsfiddle.net/4EjXd/>
