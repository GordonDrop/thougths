### this keyword

1. If the new keyword is used when calling the function, this inside the function is a brand new object.
2. If apply, call, or bind are used to call/create a function, this inside the function is the object that is passed in as the argument.
3. If a function is called as a method, such as obj.method() — this is the object that the function is a property of.
4. If a function is invoked as a free function invocation, meaning it was invoked without any of the conditions present above, this is the global object. In a browser, it is the window object. If in strict mode ('use strict'), this will be undefined instead of the global object.
5. If multiple of the above rules apply, the rule that is higher wins and will set the this value.
6. If the function is an ES2015 arrow function, it ignores all the rules above and receives the this value of its surrounding scope at the time it is created.

[Source](https://codeburst.io/the-simple-rules-to-this-in-javascript-35d97f31bde3)

### Why function foo(){ }(); is not working as IIFE?

Первая часть выражения `foo(){ }` воспринимается парсером как function declaration, вторая как вызов функции, но имя функции не указано, => будет ошибка Syntax Error.

Нужно превратить выражание в function expression `(function foo(){ })()` или `(function foo(){ }())`.

### difference between a variable that is: null, undefined or undeclared?

**Undeclared**  без let, var или const. В strict режиме это Reference error.

**undefined** объявлена без значения, также возврщает функция без явно return. Для проверки используй === или typeof.

**null** явное присвоение значения. Означает нет значения. Для проверки используй ===.

 ### use case for anonymous functions
 
- своя область видимости
- коллбек как одноразовая функция

### feature detection, feature inference, and using the UA string

**feature detection** - Детект поддерживает ли браузер ту или иную фичу.

**Feature Inference** - детект набора фич по другой фиче, предполагая, что если есть одна фича, то есть и другие искомые.

**User Agent string** - заголовок, который определяет бразуер, ОС, девайс и тд.

### 'use strict'

Advantages:

- Makes it impossible to accidentally create global variables.
- Makes assignments which would otherwise silently fail to throw an exception.
- Makes attempts to delete undeletable properties throw (where before the attempt would simply have no effect).
- Requires that function parameter names be unique.
this is undefined in the global context.
- It catches some common coding bloopers, throwing exceptions.
- It disables features that are confusing or poorly thought out.

Disadvantages:

- Many missing features that some developers might be used to.
- No more access to function.caller and function.arguments.
- Concatenation of scripts written in different strict modes might cause issues.

###  use case for the new arrow => function syntax

- не нужен контекст у самой функции
- функция, которая использует внешний контекст

### higher-order function

higher-order function is any function that takes one or more functions as arguments, which it uses to operate on some data, and/or returns a function as a result. Abstraction for repeated operations.

## type conversions

ToString - String conversion is mostly obvious. A false becomes "false", null becomes "null", etc.
           
ToNumber - мат операции:

 Ex: "6" / "2" // 3
 
 undefined -> NaN
 null -> 0
 true and false -> 1 and 0
 string - Number | NaN
 
 ToBoolean
- falsy: 0, null, undefined, NaN
- true: other

## type comparision

When comparing values of different types, JavaScript converts the values to numbers:

'2' > 1 // 2 > 1 true
'01' == 1 // 1 == 1 true 
