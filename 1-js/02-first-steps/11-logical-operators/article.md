# عملگرهای منطقی

سه نوع عملگر منطقی در جاوااسکریپت داریم: `||` (یا), `&&` (و), `!` (نقیض).

با اینکه این عملگرها منطقی نامیده میشوند، نه تنها روی داده های Boolean بلکه روی داده هایی با انواع دیگر هم عمل می کنند. همچنین خروجی آن نیز می تواند هر نوع داده ای باشد.

بیایید نگاهی به جزییات بیاندازیم.

## || (یا)

عملگر "یا" با دو کاراکتر خط عمودی نشان داده می شود:

```js
result = a || b;
```

در برنامه نویسی کلاسیک، "یا"ی منطقی تنها برای کار با داده های Boolean بکار میرود. اگر یکی از آرگومان ها مقدار true داشته باشد، این عملگر مقدار true را و در غیر اینصورت (اگر هیچیک از آرگومان ها true نباشد) مقدار false را بر میگرداند.

در جاواسکریپت، این عملگر کمی ماهرانه تر و قدرتمند تر است. ابتدا اجازه دهید ببینیم روی مقادیر Boolean چگونه کار میکند.

در کل چهار ترکیب منطقی وجود دارد:

```js run
alert( true || true );   // true
alert( false || true );  // true
alert( true || false );  // true
alert( false || false ); // false
```

همانگونه که می بینیم نتیجه همیشه true است، غیر از زمانی که هر دو عملوند (گزاره منطقی) برابر با false (نادرست) باشند.

اگر یک عملوند Boolean نباشد، برای محاسبه تبدیل به نوع Boolean می شود.

برای نمونه، عدد یک بعنوان true و عدد صفر یک گزاره false ارزیابی خواهد شد:

```js run
if (1 || 0) { // works just like if( true || false )
  alert( 'truthy!' );
}
```

بیشتر اوقات، "یا"ی منطقی `||` در دستور if بکار می رود تا درست بودن یکی از شروط را بررسی کند.

به عنوان نمونه:

```js run
let hour = 9;

*!*
if (hour < 10 || hour > 18) {
*/!*
  alert( 'The office is closed.' );
}
```

میتوانیم از شروط بیشتری هم استفاده کنیم:

```js run
let hour = 12;
let isWeekend = true;

if (hour < 10 || hour > 18 || isWeekend) {
  alert( 'The office is closed.' ); // it is the weekend
}
```

## "یا"ی منطقی اولین عبارت true را می یابد

منطقی که در بالا شرح داده شد روش کلاسیک بود. بیایید نکات بیشتری که در جاوااسکریپت وجود دارد را مطرح کنیم.

الگوریتم عملکرد "یا"ی منطقی در جاوااسکریپت اینگونه است:

فرض کنید در یک خط از چند "یا"ی منطقی استفاده شده است:

```js
result = value1 || value2 || value3;
```

عملگر `||` عملیات زیر را انجام می دهد:

- Evaluates operands from left to right.
- For each operand, converts it to boolean. If the result is `true`, stops and returns the original value of that operand.
- If all operands have been evaluated (i.e. all were `false`), returns the last operand.

A value is returned in its original form, without the conversion.

In other words, a chain of OR `"||"` returns the first truthy value or the last one if no such value is found.

For instance:

```js run
alert( 1 || 0 ); // 1 (1 is truthy)
alert( true || 'no matter what' ); // (true is truthy)

alert( null || 1 ); // 1 (1 is the first truthy value)
alert( null || 0 || 1 ); // 1 (the first truthy value)
alert( undefined || null || 0 ); // 0 (all falsy, returns the last value)
```

This leads to some interesting usage compared to a "pure, classical, boolean-only OR".

1. **Getting the first truthy value from a list of variables or expressions.**

    Imagine we have several variables which can either contain data or be `null/undefined`. How can we find the first one with data?

    We can use OR `||`:

    ```js run
    let currentUser = null;
    let defaultUser = "John";

    *!*
    let name = currentUser || defaultUser || "unnamed";
    */!*

    alert( name ); // selects "John" – the first truthy value
    ```

    If both `currentUser` and `defaultUser` were falsy, `"unnamed"` would be the result.
2. **Short-circuit evaluation.**

    Operands can be not only values, but arbitrary expressions. OR evaluates and tests them from left to right. The evaluation stops when a truthy value is reached, and the value is returned. This process is called "a short-circuit evaluation" because it goes as short as possible from left to right.

    This is clearly seen when the expression given as the second argument has a side effect like a variable assignment.

    In the example below, `x` does not get assigned:

    ```js run no-beautify
    let x;

    *!*true*/!* || (x = 1);

    alert(x); // undefined, because (x = 1) not evaluated
    ```

    If, instead, the first argument is `false`, `||` evaluates the second one, thus running the assignment:

    ```js run no-beautify
    let x;

    *!*false*/!* || (x = 1);

    alert(x); // 1
    ```

    An assignment is a simple case. Other side effects can also be involved.

    As we can see, such a use case is a "shorter way of doing `if`". The first operand is converted to boolean. If it's false, the second one is evaluated.

    Most of time, it's better to use a "regular" `if` to keep the code easy to understand, but sometimes this can be handy.

## && (AND)

The AND operator is represented with two ampersands `&&`:

```js
result = a && b;
```

In classical programming, AND returns `true` if both operands are truthy and `false` otherwise:

```js run
alert( true && true );   // true
alert( false && true );  // false
alert( true && false );  // false
alert( false && false ); // false
```

An example with `if`:

```js run
let hour = 12;
let minute = 30;

if (hour == 12 && minute == 30) {
  alert( 'The time is 12:30' );
}
```

Just as with OR, any value is allowed as an operand of AND:

```js run
if (1 && 0) { // evaluated as true && false
  alert( "won't work, because the result is falsy" );
}
```


## AND finds the first falsy value

Given multiple AND'ed values:

```js
result = value1 && value2 && value3;
```

The AND `&&` operator does the following:

- Evaluates operands from left to right.
- For each operand, converts it to a boolean. If the result is `false`, stops and returns the original value of that operand.
- If all operands have been evaluated (i.e. all were truthy), returns the last operand.

In other words, AND returns the first falsy value or the last value if none were found.

The rules above are similar to OR. The difference is that AND returns the first *falsy* value while OR returns the first *truthy* one.

Examples:

```js run
// if the first operand is truthy,
// AND returns the second operand:
alert( 1 && 0 ); // 0
alert( 1 && 5 ); // 5

// if the first operand is falsy,
// AND returns it. The second operand is ignored
alert( null && 5 ); // null
alert( 0 && "no matter what" ); // 0
```

We can also pass several values in a row. See how the first falsy one is returned:

```js run
alert( 1 && 2 && null && 3 ); // null
```

When all values are truthy, the last value is returned:

```js run
alert( 1 && 2 && 3 ); // 3, the last one
```

````smart header="Precedence of AND `&&` is higher than OR `||`"
The precedence of AND `&&` operator is higher than OR `||`.

So the code `a && b || c && d` is essentially the same as if the `&&` expressions were in parentheses: `(a && b) || (c && d)`.
````

Just like OR, the AND `&&` operator can sometimes replace `if`.

For instance:

```js run
let x = 1;

(x > 0) && alert( 'Greater than zero!' );
```

The action in the right part of `&&` would execute only if the evaluation reaches it. That is, only if `(x > 0)` is true.

So we basically have an analogue for:

```js run
let x = 1;

if (x > 0) {
  alert( 'Greater than zero!' );
}
```

The variant with `&&` appears shorter. But `if` is more obvious and tends to be a little bit more readable.

So we recommend using every construct for its purpose: use `if` if we want if and use `&&` if we want AND.

## ! (NOT)

The boolean NOT operator is represented with an exclamation sign `!`.

The syntax is pretty simple:

```js
result = !value;
```

The operator accepts a single argument and does the following:

1. Converts the operand to boolean type: `true/false`.
2. Returns the inverse value.

For instance:

```js run
alert( !true ); // false
alert( !0 ); // true
```

A double NOT `!!` is sometimes used for converting a value to boolean type:

```js run
alert( !!"non-empty string" ); // true
alert( !!null ); // false
```

That is, the first NOT converts the value to boolean and returns the inverse, and the second NOT inverses it again. In the end, we have a plain value-to-boolean conversion.

There's a little more verbose way to do the same thing -- a built-in `Boolean` function:

```js run
alert( Boolean("non-empty string") ); // true
alert( Boolean(null) ); // false
```

The precedence of NOT `!` is the highest of all logical operators, so it always executes first, before `&&` or `||`.
