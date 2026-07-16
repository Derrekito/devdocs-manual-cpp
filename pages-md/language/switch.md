# switch statement

Transfers control to one of several statements, depending on the value of a
condition.

### Syntax

```text
attr ﻿(optional) switch ( init-statement ﻿(optional) condition ) statement
```

- **attr** — (since C++11) any number of attributes
- **init-statement** — (since C++17) any of the following: an expression
  statement (which may be a *null statement* "`;`") a simple declaration,
  typically a declaration of a variable with initializer, but it may declare
  arbitrarily many variables or structured bindings an alias declaration (since
  C++23) Note that any init-statement must end with a semicolon `;`, which is
  why it is often described informally as an expression or a declaration
  followed by a semicolon.
- **an alias declaration** — (since C++23)
- **condition** — any of the following: an expression, in this case the value of
  condition is the value of the expression a declaration of a single non-array
  variable of such type with a brace-or-equals initializer, in this case the
  value of condition is the value of the declared variable The value of
  condition must be of integral or enumeration type, or of a class type
  contextually implicitly convertible to an integral or enumeration type. If the
  (possibly converted) type is subject to integral promotions, condition is
  converted to the promoted type.
- **statement** — any statement (typically a compound statement). `case:` and
  `default:` labels are permitted in statement and `break;` statement has
  special meaning.

```text
attr ﻿(optional) case constant-expression : statement    (1)
attr ﻿(optional) default : statement    (2)
```

- **constant-expression** — a constant expression of the same type as the type
  of condition after conversions and integral promotions

### Explanation

The body of a switch statement may have an arbitrary number of `case:` labels,
as long as the values of all constant-expressions are unique (after
conversions/promotions). At most one `default:` label may be present (although
nested switch statements may use their own `default:` labels or have `case:`
labels whose constants are identical to the ones used in the enclosing switch).

If condition evaluates to a value that is equal to the value of one of
constant-expressions, then control is transferred to the statement that is
labeled with that constant-expression.

If condition evaluates to a value that doesn't match any of the `case:` labels,
and the `default:` label is present, control is transferred to the statement
labeled with the `default:` label.

If condition evaluates to a value that doesn't match any of the `case:` labels,
and the `default:` label is not present, then none of the statements in the
switch body is executed.

The break statement, when encountered in statement exits the switch statement:

```cpp
switch (1)
{
    case 1:
        std::cout << '1'; // prints "1",
    case 2:
        std::cout << '2'; // then prints "2"
}
```

```cpp
switch (1)
{
    case 1:
        std::cout << '1'; // prints "1"
        break;            // and exits the switch
    case 2:
        std::cout << '2';
        break;
}
```

Compilers may issue warnings on fallthrough (reaching the next case label
without a break) unless the attribute `[[fallthrough]]` appears immediately
before the case label to indicate that the fallthrough is intentional.
If init-statement is used, the switch statement is equivalent to
```text
{ init-statement switch ( condition ) statement }
```
Except that names declared by the init-statement (if init-statement is a
declaration) and names declared by condition (if condition is a declaration) are
in the same scope, which is also the scope of statement.
*(since C++17)*

Because transfer of control is not permitted to enter the scope of a variable,
if a declaration statement is encountered inside the statement, it has to be
scoped in its own compound statement:

```cpp
switch (1)
{
    case 1:
        int x = 0; // initialization
        std::cout << x << '\n';
        break;
    default:
        // compilation error: jump to default:
        // would enter the scope of 'x' without initializing it
        std::cout << "default\n";
        break;
}
```

```cpp
switch (1)
{
    case 1:
        {
            int x = 0;
            std::cout << x << '\n';
            break;
        } // scope of 'x' ends here
    default:
        std::cout << "default\n"; // no error
        break;
}
```

### Keywords

`switch`, `case`, `default`

### Example

The following code shows several usage cases of the *switch* statement

```cpp
#include <iostream>

int main()
{
    const int i = 2;
    switch (i)
    {
        case 1:
            std::cout << '1';
        case 2:              // execution starts at this case label
            std::cout << '2';
        case 3:
            std::cout << '3';
            [[fallthrough]]; // C++17 attribute to silent the warning on fallthrough
        case 5:
            std::cout << "45";
            break;           // execution of subsequent statements is terminated
        case 6:
            std::cout << '6';
    }

    std::cout << '\n';

    switch (i)
    {
        case 4:
            std::cout << 'a';
        default:
            std::cout << 'd'; // there are no applicable constant expressions
                              // therefore default is executed
    }

    std::cout << '\n';

    switch (i)
    {
        case 4:
            std::cout << 'a'; // nothing is executed
    }

    // when enumerations are used in a switch statement, many compilers
    // issue warnings if one of the enumerators is not handled
    enum color { RED, GREEN, BLUE };
    switch (RED)
    {
        case RED:
            std::cout << "red\n";
            break;
        case GREEN:
            std::cout << "green\n";
            break;
        case BLUE:
            std::cout << "blue\n";
            break;
    }

    // the C++17 init-statement syntax can be helpful when there is
    // no implicit conversion to integral or enumeration type
    struct Device
    {
        enum State { SLEEP, READY, BAD };
        auto state() const { return m_state; }

        /*...*/

    private:
        State m_state{};
    };

    switch (auto dev = Device{}; dev.state())
    {
        case Device::SLEEP:
            /*...*/
            break;
        case Device::READY:
            /*...*/
            break;
        case Device::BAD:
            /*...*/
            break;
    }

    // pathological examples

    // the statement doesn't have to be a compound statement
    switch (0)
        std::cout << "this does nothing\n";

    // labels don't require a compound statement either
    switch (int n = 1)
    {
        case 0:
        case 1:
            std::cout << n << '\n';
    }
}
```

Output:

```text
2345
d
red
1
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 1767 | C++98 | conditions of types that are not subject to integral
      promotion could not be promoted | do not promote conditions of these types
  CWG 2629 | C++98 | condition could be a declaration of a floating-point
      variable | prohibited

### See also

**C documentation for `switch`**

### External links

  1. | Loop unrolling using Duff's Device
  2. | Duff's device can be used to implement coroutines in C/C++

---
*Source: https://en.cppreference.com/w/cpp/language/switch*
