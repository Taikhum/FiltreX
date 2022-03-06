Filtrex
=======

A simple, safe, JavaScript expression engine, allowing end-users to enter arbitrary boolean algebra expressions and converting them to an filter tree which can easily be 
used to query a data base or create beautiful visualizations as show below.

<img width="449" alt="image" src="https://user-images.githubusercontent.com/47829739/156943672-98adbfd0-5ce6-4e43-b31c-c822f7a3cd1f.png">

Built using JISON, a JavaScript parser generator based of on Flex and Bison

```python
AI and impact and (ethics or ethical or Ai ethics or “artifical intelligence ethiscs”) and not (green ethics or WECAN)
```

Sure, you could do that with vanilla JavaScript and `eval()`, but that is very unsafe as it can be used to launch cross-site scripting (XSS) attacks or SQL injection.

Filtrex defines a really simple expression language that should be familiar to anyone who's ever used a spreadsheet and compile it into a JavaScript function at runtime.

example of a filter tree

<img width="792" alt="image" src="https://user-images.githubusercontent.com/47829739/156943634-020e3f9f-5598-4b8e-87ad-68372fafd92f.png">

Features
--------

*   **Simple!** Very easy to adapt to ur requirements
*   **Fast!** Expressions get compiled into JavaScript functions, offering the same performance as if it had been hand coded. e.g. 
*   **Safe!** You as the developer have control of which data can be accessed and the functions that can be called. Expressions cannot escape the sandbox.
*   **Pluggable!** Add your own data and functions.
*   **Predictable!** Because users can't define loops or recursive functions, you know you won't be left hanging.


Under the hood, the above expression gets compiled to a clean and fast Filter Tree, looking something like this:


Expressions
-----------

There are only 2 types: numbers and strings. Numbers may be floating point or integers. Boolean logic is applied on the truthy value of values (e.g. any non-zero number is true, any non-empty string is true, otherwise false).

Values | Description
--- | ---
43, -1.234 | Numbers
"hello" | String
foo, a.b.c, 'foo-bar' | External data variable defined by application (may be numbers or strings)

Numeric arithmetic | Description
--- | ---
x + y | Add
x - y | Subtract
x * y | Multiply
x / y | Divide
x % y | Modulo
x ^ y | Power

Comparisons | Description
--- | ---
x == y | Equals
x < y | Less than
x <= y | Less than or equal to
x > y | Greater than
x >= y | Greater than or equal to
x ~= y | Regular expression match
x in (a, b, c) | Equivalent to (x == a or x == b or x == c)
x not in (a, b, c) | Equivalent to (x != a and x != b and x != c)

Boolean logic | Description
--- | ---
x or y | Boolean or
x and y | Boolean and
not x | Boolean not
x ? y : z | If boolean x, value y, else z
( x ) | Explicity operator precedence

Built-in functions | Description
--- | ---
abs(x) | Absolute value
ceil(x) | Round floating point up
floor(x) | Round floating point down
log(x) | Natural logarithm
max(a, b, c...) | Max value (variable length of args)
min(a, b, c...) | Min value (variable length of args)
random() | Random floating point from 0.0 to 1.0
round(x) | Round floating point
sqrt(x) | Square root

Operator precedence follows that of any sane language.

Adding custom functions
-----------------------

When integrating in to your application, you can add your own custom functions.

```javascript
// Custom function: Return string length.
function strlen(s) {
  return s.length;
}

// Compile expression to executable function
var myfilter = compileExpression(
                    'strlen(firstname) > 5',
                    {strlen:strlen}); // custom functions

myfilter({firstname:'Joe'});    // returns 0
myfilter({firstname:'Joseph'}); // returns 1
```

FAQ
---

**Why the name?**

Because it was originally built for FILTeR EXpressions.

**What's Jison?**

[Jison](http://zaach.github.io/jison/) is bundled with Filtrex – it's a JavaScript parser generator that does the underlying hard work of understanding the expression. It's based on Flex and Bison.


**What happens if the expression is malformed?**

Calling `compileExpression()` with a malformed expression will throw an exception. You can catch that and display feedback to the user. A good UI pattern is to attempt to compile on each keystroke and continuously indicate whether the expression is valid.

