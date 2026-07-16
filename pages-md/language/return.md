# `return` statement

Terminates the current function and returns the specified value (if any) to the
caller.

### Syntax

```text
attr ﻿(optional) return expression ﻿(optional) ;    (1)
attr ﻿(optional) return braced-init-list ;    (2)    (since C++11)
attr ﻿(optional) co_return expression ﻿(optional) ;    (3)    (since C++20)
attr ﻿(optional) co_return braced-init-list ;    (4)    (since C++20)
```

- **attr** — (since C++11) sequence of any number of attributes
- **expression** — expression, convertible to the function return type
- **braced-init-list** — brace-enclosed list of initializers and other
  braced-init-lists

### Explanation

1) Evaluates the expression, terminates the current function and returns the
   result of the expression to the caller, after implicit conversion to the
   function return type. The expression is optional in functions whose return
   type is (possibly cv-qualified) `void`, and disallowed in constructors and in
   destructors.

2) Uses copy-list-initialization to construct the return value of the function.

3,4) In a coroutine, the keyword `co_return` must be used instead of `return`
   for the final suspension point (see coroutines for details).

There is a sequence point between the copy-initialization of the result of the
function call and the destruction of all temporaries at the end of expression.
*(until C++11)*

The copy-initialization of the result of the function call is sequenced-before
the destruction of all temporaries at the end of expression, which, in turn, is
*sequenced-before* the destruction of local variables of the block enclosing the
return statement.
*(since C++11)*

### Notes

If control reaches the end of

- a function with the return type (possibly cv-qualified) `void`,
- a constructor,
- a destructor, or
- a function-try-block for a function with the return type (possibly
  cv-qualified) `void`

without encountering a return statement, `return;` is executed.

If control reaches the end of the main function, `return 0;` is executed.

Flowing off the end of a value-returning function, except `main` and specific
coroutines(since C++20), without a return statement is undefined behavior.

In a function returning (possibly cv-qualified) `void`, the return statement
with expression can be used, if the expression type is (possibly cv-qualified)
`void`.

If the return type of a function is specified as a placeholder type, it will be
deduced from the return value.
*(since C++14)*

Returning by value may involve construction and copy/move of a temporary object,
unless copy elision is used. Specifically, the conditions for copy/move are as
follows:

#### Automatic move from local variables and parameters
The expression is *move-eligible* if it is a (possibly parenthesized)
id-expression that names a variable of automatic storage duration whose type is
- a non-volatile object type
- or a non-volatile rvalue reference to object type
*(since C++20)*
and that variable is declared
  - in the body or
  - as a parameter
  of the innermost enclosing function or lambda expression.
If the expression is move-eligible, overload resolution to select the
constructor to use for initialization of the returned value or, for `co_return`,
to select the overload of `promise.return_value()`(since C++20) is performed
*twice*:
- first as if expression were an rvalue expression (thus it may select the move
  constructor), and
- it succeeded, but did not select the move constructor (formally, the first
  parameter of the selected constructor was not an rvalue reference to the
  (possibly cv-qualified) type of expression)
*(until C++20)*
- then overload resolution is performed as usual, with expression considered as
  an lvalue (so it may select the copy constructor).
*(until C++23)*
- it succeeded, but did not select the move constructor (formally, the first
  parameter of the selected constructor was not an rvalue reference to the
  (possibly cv-qualified) type of expression)
*(until C++20)*
If the expression is move-eligible, it is treated as an xvalue (thus overload
resolution may select the move constructor).
*(since C++23)*
*(since C++11)*

- or a non-volatile rvalue reference to object type
*(since C++20)*

If the expression is move-eligible, overload resolution to select the
constructor to use for initialization of the returned value or, for `co_return`,
to select the overload of `promise.return_value()`(since C++20) is performed
*twice*:
- first as if expression were an rvalue expression (thus it may select the move
  constructor), and
- it succeeded, but did not select the move constructor (formally, the first
  parameter of the selected constructor was not an rvalue reference to the
  (possibly cv-qualified) type of expression)
*(until C++20)*
- then overload resolution is performed as usual, with expression considered as
  an lvalue (so it may select the copy constructor).
*(until C++23)*

- it succeeded, but did not select the move constructor (formally, the first
  parameter of the selected constructor was not an rvalue reference to the
  (possibly cv-qualified) type of expression)
*(until C++20)*

If the expression is move-eligible, it is treated as an xvalue (thus overload
resolution may select the move constructor).
*(since C++23)*

#### Guaranteed copy elision
If expression is a prvalue, the result object is initialized directly by that
expression. This does not involve a copy or move constructor when the types
match (see copy elision).
*(since C++17)*

  Feature-test macro | Value | Std | Feature
  `__cpp_implicit_move` | 202207L | (C++23) | Simpler implicit move

### Keywords

`return`, `co_return`

### Example

```cpp
#include <iostream>
#include <string>
#include <utility>

void fa(int i)
{
    if (i == 2)
        return;
    std::cout << "fa("<< i << ")\n";
} // implied return;

int fb(int i)
{
    if (i > 4)
        return 4;
    std::cout << "fb(" << i << ")\n";
    return 2;
}

std::pair<std::string, int> fc(const char* p, int x)
{
    return {p, x};
}

void fd()
{
    return fa(10); // fa(10) is a void expression
}

int main()
{
    fa(1); // prints its argument, then returns
    fa(2); // does nothing when i == 2, just returns

    int i = fb(5); // returns 4
    i = fb(i);     // prints its argument, returns 2
    std::cout << "i = " << i << '\n'
              << "fc(~).second = " << fc("Hello", 7).second << '\n';

    fd();
}
```

Output:

```text
fa(1)
fb(4)
i = 2
fc(~).second = 7
fa(10)
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 1541 | C++98 | expression could not be omitted if the return type is
      cv-qualified `void` | it can be omitted
  CWG 1579 | C++11 | return by converting move constructor was not allowed |
      converting move constructor lookup enabled
  CWG 1885 | C++98 | sequencing of the destruction of automatic variables was
      not explicit | sequencing rules added

### See also

- copy elision

**C documentation for `return` statement**

---
*Source: https://en.cppreference.com/w/cpp/language/return*
