# Zero-initialization

Sets the initial value of an object to zero.

### Syntax

Note that this is not the syntax for zero-initialization, which does not have a
dedicated syntax in the language. These are examples of other types of
initializations, which might perform zero-initialization.

```text
static T object ;    (1)
T () ; T t = {} ; T {} ; (since C++11)    (2)
CharT array [ n ] = " short-sequence ";    (3)
```

### Explanation

Zero-initialization is performed in the following situations:

1) For every named variable with static or thread-local(since C++11) storage
   duration that is not subject to constant initialization, before any other
   initialization.

2) As part of value-initialization sequence for non-class types and for members
   of value-initialized class types that have no constructors, including value
   initialization of elements of aggregates for which no initializers are
   provided.

3) When an array of any character type is initialized with a string literal that
   is too short, the remainder of the array is zero-initialized.

The effects of zero-initialization are:

- If `T` is a scalar type, the object is initialized to the value obtained by
  explicitly converting the integer literal `​0​` (zero) to `T`.
- If `T` is a non-union class type:
- If `T` is a union type:
- If `T` is array type, each element is zero-initialized.
- If `T` is reference type, nothing is done.

### Notes

As described in non-local initialization, static and thread-local(since C++11)
variables that aren't constant-initialized are zero-initialized before any other
initialization takes place. If the definition of a non-class non-local variable
has no initializer, then default initialization does nothing, leaving the result
of the earlier zero-initialization unmodified.

A zero-initialized pointer is the null pointer value of its type, even if the
value of the null pointer is not integral zero.

### Example

```cpp
#include <iostream>
#include <string>

struct A
{
    int a, b, c;
};

double f[3];   // zero-initialized to three 0.0's

int* p;        // zero-initialized to null pointer value
               // (even if the value is not integral 0)

std::string s; // zero-initialized to indeterminate value, then
               // default-initialized to "" by the std::string default constructor

int main(int argc, char*[])
{
    delete p; // safe to delete a null pointer

    static int n = argc; // zero-initialized to 0 then copy-initialized to argc
    std::cout << "n = " << n << '\n';

    A a = A(); // the effect is same as: A a{}; or A a = {};
    std::cout << "a = {" << a.a << ' ' << a.b << ' ' << a.c << "}\n";
}
```

Possible output:

```text
n = 1
a = {0 0 0}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 277 | C++98 | pointers might be initialized with a non-constant expression
      of value 0, which is not a null pointer constant | must initialize with an
      integral constant expression of value 0
  CWG 694 | C++98 | zero-initialization for class types ignored padding |
      padding is initialized to zero bits
  CWG 903 | C++98 | zero-initialization for scalar types set the initial value
      to the value converted from an integral constant expression with value 0 |
      the object is initialized to the value converted from the integer literal
      `​0​`
  CWG 2026 | C++98 | zero-initialization was specified to always occur first,
      even before constant initialization | no zero-initialization if constant
      initialization applies
  CWG 2196 | C++98 | zero-initialization for class types ignored base class
      subobjects | they are also zero-initialized
  CWG 2253 | C++98 | it was unclear whether zero-initialization applies to
      unnamed bit-fields | it applies (all padding bits are initialized to zero
      bits)

### See also

- constructor
- copy assignment
- default constructor
- initialization aggregate initialization constant initialization copy
  initialization default initialization direct initialization list
  initialization value initialization
- move assignment
- `new`

---
*Source: https://en.cppreference.com/w/cpp/language/zero_initialization*
