# C++ attribute: assume (since C++23)

Specifies that an expression will always evaluate to `true` at a given point.

### Syntax

```text
[[assume( expression )]]
```

- **expression** — expression that must evaluate to `true`

### Explanation

Can only be applied to a null statement, as in `[[assume(x > 0)]];`. This
statement is called an *assumption*. If the expression (contextually converted
to `bool`) would not evaluate to `true` at the place the assumption occurs, the
behavior is undefined. Otherwise, the statement does nothing. In particular, the
expression is not evaluated (but it is still potentially evaluated).

The purpose of an assumption is to allow compiler optimizations based on the
information given.

The expression may not be a comma operator expression, but enclosing the
expression in parentheses will allow the comma operator to be used.

### Notes

If the expression would have undefined behavior, or if it would cause an
exception to be thrown, then it does not evaluate to `true`.

Since assumptions cause undefined behavior if they do not hold, they should be
used sparingly. They are not intended as a mechanism to document the
preconditions of a function or to diagnose violations of preconditions. Also, it
should not be presumed, without checking, that the compiler actually makes use
of any particular assumption.

### Example

```cpp
void f(int& x, int y)
{
    void g(int);
    void h();

    [[assume(x > 0)]]; // Compiler may assume x is positive

    g(x / 2); // More efficient code possibly generated

    x = 3;
    int z = x;

    [[assume((h(), x == z))]]; // Compiler may assume x would have the same value after
                               // calling h
                               // The assumption does not cause a call to h

    h();
    g(x); // Compiler may replace this with g(3);

    h();
    g(x); // Compiler may NOT replace this with g(3);
          // An assumption applies only at the point where it appears

    z = std::abs(y);

    [[assume((g(z), true))]]; // Compiler may assume g(z) will return

    g(z); // Due to above and below assumptions, compiler may replace this with g(10);

    [[assume(y == -10)]]; // Undefined behavior if y != -10 at this point

    [[assume((x - 1) * 3 == 12)]];

    g(x); // Compiler may replace this with g(5);
}
```

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

- **unreachable (C++23)** — marks unreachable point of execution (function)

### External links

  1. | Clang language extensions doc: `__builtin_assume`.
  2. | Clang attribute reference doc: `assume`.
  3. | MSVC doc: `__assume` built-in.
  4. | GCC doc: `__attribute__((assume(...)))`.

---
*Source: https://en.cppreference.com/w/cpp/language/attributes/assume*
