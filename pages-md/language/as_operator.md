# Copy assignment operator

A copy assignment operator is a non-template non-static member function with the
name operator= that can be called with an argument of the same class type and
copies the content of the argument without mutating the argument.

### Syntax

For the formal copy assignment operator syntax, see function declaration. The
syntax list below only demonstrates a subset of all valid copy assignment
operator syntaxes.

```text
return-type operator=(parameter-list ﻿);    (1)
return-type operator=(parameter-list ﻿) function-body    (2)
return-type operator=(parameter-list-no-default ﻿) = default;    (3)    (since C++11)
return-type operator=(parameter-list ﻿) = delete;    (4)    (since C++11)
return-type class-name ﻿::operator=(parameter-list ﻿) function-body    (5)
return-type class-name ﻿::operator=(parameter-list-no-default ﻿) = default;    (6)    (since C++11)
```

- **class-name** — the class whose copy assignment operator is being declared,
  the class type is given as `T` in the descriptions below
- **parameter-list** — a parameter list of only one parameter, which is of type
  `T`, `T&`, const T&, volatile T& or const volatile T&
- **parameter-list-no-default** — a parameter list of only one parameter, which
  is of type `T`, `T&`, const T&, volatile T& or const volatile T& and does not
  have a default argument
- **function-body** — the function body of the copy assignment operator
- **return-type** — any type, but `T&` is favored in order to allow chaining
  asssignments

### Explanation

1) Declaration of a copy assignment operator inside of class definition.

2-4) Definition of a copy assignment operator inside of class definition.

   3) The copy assignment operator is explicitly-defaulted.

   4) The copy assignment operator is deleted.

5,6) Definition of a copy assignment operator outside of class definition (the
   class must contain a declaration (1)).

   6) The copy assignment operator is explicitly-defaulted.

```cpp
struct X
{
    X& operator=(X& other);     // copy assignment operator
    X operator=(X other);       // pass-by-value is allowed
//  X operator=(const X other); // Error: incorrect parameter type
};

union Y
{
    // copy assignment operators can have syntaxes not listed above,
    // as long as they follow the general function declaration syntax
    // and do not viloate the restrictions listed above
    auto operator=(Y& other) -> Y&;       // OK: trailing return type
    Y& operator=(this Y& self, Y& other); // OK: explicit object parameter
//  Y& operator=(Y&, int num = 1);        // Error: has other non-object parameters
};
```

The copy assignment operator is called whenever selected by overload resolution,
e.g. when an object appears on the left side of an assignment expression.

### Implicitly-declared copy assignment operator

If no user-defined copy assignment operators are provided for a class type, the
compiler will always declare one as an inline public member of the class. This
implicitly-declared copy assignment operator has the form `T& T::operator=(const
T&)` if all of the following is true:

- each direct base `B` of `T` has a copy assignment operator whose parameters
  are `B` or const B& or const volatile B&;
- each non-static data member `M` of `T` of class type or array of class type
  has a copy assignment operator whose parameters are `M` or const M& or const
  volatile M&.

Otherwise the implicitly-declared copy assignment operator is declared as `T&
T::operator=(T&)`.

Due to these rules, the implicitly-declared copy assignment operator cannot bind
to a volatile lvalue argument.

A class can have multiple copy assignment operators, e.g. both `T&
T::operator=(T&)` and `T& T::operator=(T)`. If some user-defined copy assignment
operators are present, the user may still force the generation of the implicitly
declared copy assignment operator with the keyword `default`.(since C++11)

The implicitly-declared (or defaulted on its first declaration) copy assignment
operator has an exception specification as described in dynamic exception
specification(until C++17)noexcept specification(since C++17)

Because the copy assignment operator is always declared for any class, the base
class assignment operator is always hidden. If a using-declaration is used to
bring in the assignment operator from the base class, and its argument type
could be the same as the argument type of the implicit assignment operator of
the derived class, the using-declaration is also hidden by the implicit
declaration.

### Implicitly-defined copy assignment operator

If the implicitly-declared copy assignment operator is neither deleted nor
trivial, it is defined (that is, a function body is generated and compiled) by
the compiler if odr-used or needed for constant evaluation(since C++14). For
union types, the implicitly-defined copy assignment copies the object
representation (as by `std::memmove`). For non-union class types, the operator
performs member-wise copy assignment of the object's bases and non-static
members, in their initialization order, using built-in assignment for the
scalars and copy assignment operator for class types.

The implicitly-defined copy assignment operator for a class `T` is `constexpr`
if
- `T` is a literal type, and
- the assignment operator selected to copy each direct base class subobject is a
  constexpr function, and
- for each non-static data member of `T` that is of class type (or array
  thereof), the assignment operator selected to copy that member is a constexpr
  function.
*(since C++14)(until C++23)*

The implicitly-defined copy assignment operator for a class `T` is `constexpr`.
*(since C++23)*

The generation of the implicitly-defined copy assignment operator is deprecated
if `T` has a user-declared destructor or user-declared copy constructor.
*(since C++11)*

### Deleted copy assignment operator

An implicitly-declared or explicitly-defaulted(since C++11) copy assignment
operator for class `T` is undefined(until C++11)defined as deleted(since C++11)
if any of the following conditions is satisfied:

- `T` has a non-static data member of a const-qualified non-class type (or
  possibly multi-dimensional array thereof).
- `T` has a non-static data member of a reference type.
- `T` has a potentially constructed subobject of class type `M` (or possibly
  multi-dimensional array thereof) such that the overload resolution as applied
  to find `M`'s copy assignment operator

The implicitly-declared copy assignment operator for class `T` is defined as
deleted if `T` declares a move constructor or move assignment operator.
*(since C++11)*

### Trivial copy assignment operator

The copy assignment operator for class `T` is trivial if all of the following is
true:

- it is not user-provided (meaning, it is implicitly-defined or defaulted);
- `T` has no virtual member functions;
- `T` has no virtual base classes;
- the copy assignment operator selected for every direct base of `T` is trivial;
- the copy assignment operator selected for every non-static class type (or
  array of class type) member of `T` is trivial.

A trivial copy assignment operator makes a copy of the object representation as
if by `std::memmove`. All data types compatible with the C language (POD types)
are trivially copy-assignable.

### Eligible copy assignment operator

A copy assignment operator is eligible if it is either user-declared or both
implicitly-declared and definable.
*(until C++11)*

A copy assignment operator is eligible if it is not deleted.
*(since C++11)(until C++20)*

A copy assignment operator is eligible if
- it is not deleted, and
- its associated constraints, if any, are satisfied, and
- no copy assignment operator with the same first parameter type and the same
  cv/ref-qualifiers (if any) is more constrained than it.
*(since C++20)*

Triviality of eligible copy assignment operators determines whether the class is
a trivially copyable type.

### Notes

If both copy and move assignment operators are provided, overload resolution
selects the move assignment if the argument is an rvalue (either a prvalue such
as a nameless temporary or an xvalue such as the result of `std::move`), and
selects the copy assignment if the argument is an lvalue (named object or a
function/operator returning lvalue reference). If only the copy assignment is
provided, all argument categories select it (as long as it takes its argument by
value or as reference to const, since rvalues can bind to const references),
which makes copy assignment the fallback for move assignment, when move is
unavailable.

It is unspecified whether virtual base class subobjects that are accessible
through more than one path in the inheritance lattice, are assigned more than
once by the implicitly-defined copy assignment operator (same applies to move
assignment).

See assignment operator overloading for additional detail on the expected
behavior of a user-defined copy-assignment operator.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <memory>
#include <string>

struct A
{
    int n;
    std::string s1;

    A() = default;
    A(A const&) = default;

    // user-defined copy assignment (copy-and-swap idiom)
    A& operator=(A other)
    {
        std::cout << "copy assignment of A\n";
        std::swap(n, other.n);
        std::swap(s1, other.s1);
        return *this;
    }
};

struct B : A
{
    std::string s2;
    // implicitly-defined copy assignment
};

struct C
{
    std::unique_ptr<int[]> data;
    std::size_t size;

    // user-defined copy assignment (non copy-and-swap idiom)
    // note: copy-and-swap would always reallocate resources
    C& operator=(const C& other)
    {
        if (this != &other) // not a self-assignment
        {
            if (size != other.size) // resource cannot be reused
            {
                data.reset(new int[other.size]);
                size = other.size;
            }
            std::copy(&other.data[0], &other.data[0] + size, &data[0]);
        }
        return *this;
    }
};

int main()
{
    A a1, a2;
    std::cout << "a1 = a2 calls ";
    a1 = a2; // user-defined copy assignment

    B b1, b2;
    b2.s1 = "foo";
    b2.s2 = "bar";
    std::cout << "b1 = b2 calls ";
    b1 = b2; // implicitly-defined copy assignment

    std::cout << "b1.s1 = " << b1.s1 << "; b1.s2 = " << b1.s2 << '\n';
}
```

Output:

```text
a1 = a2 calls copy assignment of A
b1 = b2 calls copy assignment of A
b1.s1 = foo; b1.s2 = bar
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 1353 | C++98 | the conditions where implicitly-declared copy assignment
      operators are undefined did not consider multi-dimensional array types |
      consider these types
  CWG 2094 | C++11 | a volatile subobject made defaulted copy assignment
      operators non-trivial (CWG issue 496) | triviality not affected
  CWG 2171 | C++11 | `operator=(X&) = default` was non-trivial | made trivial
  CWG 2180 | C++11 | a defaulted copy assignment operator for class `T` was not
      defined as deleted if `T` is abstract and has non-copy-assignable direct
      virtual base classes | the operator is defined as deleted in this case

### See also

- converting constructor
- copy constructor
- copy elision
- default constructor
- destructor
- `explicit`
- initialization aggregate initialization constant initialization copy
  initialization default initialization direct initialization initializer list
  list initialization reference initialization value initialization zero
  initialization
- move assignment
- move constructor
- `new`

---
*Source: https://en.cppreference.com/w/cpp/language/as_operator*
