# Constant initialization

Sets the initial values of the static variables to a compile-time constant.

### Explanation

If a static or thread-local(since C++11) variable is constant-initialized (see
below), constant initialization is performed instead of zero initialization
before all other initializations.

A variable or temporary object `obj` is *constant-initialized* if

- either it has an initializer or its default-initialization results in some
  initialization being performed, and
- its initialization full-expression is a constant expression, except that if
  `obj` is an object, that full-expression may also invoke constexpr
  constructors for `obj` and its subobjects even if those objects are of
  non-literal class types(since C++11).

The effects of constant initialization are the same as the effects of the
corresponding initialization, except that it's guaranteed that it is complete
before any other initialization of a static or thread-local(since C++11) object
begins, and it may be performed at compile time.

### Notes

The compiler is permitted to initialize other static and thread-local(since
C++11) objects using constant initialization, if it can guarantee that the value
would be the same as if the standard order of initialization was followed.

In practice, constant initialization is performed at compile time, and
pre-calculated object representations are stored as part of the program image
(e.g. in the `.data` section). If a variable is both `const` and constant
initialized, its object representation may be stored in a read-only section of
the program image (e.g. the `.rodata` section)

### Example

```cpp
#include <iostream>
#include <array>

struct S
{
    static const int c;
};

const int d = 10 * S::c; // not a constant expression: S::c has no preceding
                         // initializer, this initialization happens after const
const int S::c = 5;      // constant initialization, guaranteed to happen first

int main()
{
    std::cout << "d = " << d << '\n';
    std::array<int, S::c> a1; // OK: S::c is a constant expression
//  std::array<int, d> a2;    // error: d is not a constant expression
}
```

Output:

```text
d = 50
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 441 | C++98 | references could not be constant initialized | made constant
      initializable
  CWG 1489 | C++98 | it was unclear whether value-initializing an object can be
      a constant initialization | it can
  CWG 1747 | C++98 | binding a reference to a function could not be constant
      initialization | it can
  CWG 1834 | C++11 | binding a reference to an xvalue could not be constant
      initialization | it can
  CWG 2026 | C++98 | zero-initialization was specified to always occur first,
      even before constant initialization | no zero-initialization if constant
      initialization applies
  CWG 2366 | C++98 | default-initialization could not be constant initialization
      (constant initializers were required) | it can

### See also

- `constexpr`
- constructor
- converting constructor
- copy constructor
- default constructor
- `explicit`
- initialization aggregate initialization copy initialization default
  initialization direct initialization list initialization reference
  initialization value initialization zero initialization
- move constructor

---
*Source: https://en.cppreference.com/w/cpp/language/constant_initialization*
