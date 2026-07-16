# goto statement

Transfers control unconditionally.

Used when it is otherwise impossible to transfer control to the desired location
using other statements.

### Syntax

```text
attr ﻿(optional) goto label ;
```

### Explanation

The goto statement transfers control to the location specified by label. The
goto statement must be in the same function as the label it is referring, it may
appear before or after the label.

If transfer of control exits the scope of any automatic variables (e.g. by
jumping backwards to a point before the declarations of such variables or by
jumping forward out of a compound statement where the variables are scoped), the
destructors are called for all variables whose scope was exited, in the order
opposite to the order of their construction.

The `goto` statement cannot transfer control into a try-block or into a
catch-clause, but can transfer control out of a try-block or a catch-clause (the
rules above regarding automatic variables in scope are followed).

If transfer of control enters the scope of any automatic variables (e.g. by
jumping forward over a declaration statement), the program is ill-formed (cannot
be compiled), unless all variables whose scope is entered have

1) scalar types declared without initializers

2) class types with trivial default constructors and trivial destructors
   declared without initializers

3) cv-qualified versions of one of the above

4) arrays of one of the above

(Note: the same rules apply to all forms of transfer of control)

### Keywords

`goto`

### Notes

In the C programming language, the `goto` statement has fewer restrictions and
can enter the scope of any variable other than variable-length array or
variably-modified pointer.

### Example

```cpp
#include <iostream>

struct Object
{
    // non-trivial destructor
    ~Object() { std::cout << 'd'; }
};

struct Trivial
{
    double d1;
    double d2;
}; // trivial ctor and dtor

int main()
{
    int a = 10;

    // loop using goto
label:
    Object obj;
    std::cout << a << ' ';
    a -= 2;

    if (a != 0)
        goto label;  // jumps out of scope of obj, calls obj destructor
    std::cout << '\n';

    // goto can be used to efficiently leave a multi-level (nested) loops
    for (int x = 0; x < 3; ++x)
        for (int y = 0; y < 3; ++y)
        {
            std::cout << '(' << x << ',' << y << ") " << '\n';
            if (x + y >= 3)
                goto endloop;
        }

endloop:
    std::cout << '\n';

    goto label2; // jumps into the scope of n and t

    [[maybe_unused]] int n; // no initializer

    [[maybe_unused]] Trivial t; // trivial ctor/dtor, no initializer

//  int x = 1;   // error: has initializer
//  Object obj2; // error: non-trivial dtor

label2:
    {
        Object obj3;
        goto label3; // jumps forward, out of scope of obj3
    }

label3:
    std::cout << '\n';
}
```

Output:

```text
10 d8 d6 d4 d2
(0,0)
(0,1)
(0,2)
(1,0)
(1,1)
(1,2)

d
d
```

### See also

**C documentation for `goto`**

### External links

  The popular Edsger W. Dijkstra essay, “Goto Considered Harmful” (originally,
      in "Letter to Communications of the ACM (CACM)", vol. 11 #3, March 1968,
      pp. 147-148.), presents a survey of the many subtle problems the careless
      use of this keyword can introduce.

---
*Source: https://en.cppreference.com/w/cpp/language/goto*
