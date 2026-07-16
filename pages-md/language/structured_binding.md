# Structured binding declaration (since C++17)

Binds the specified names to subobjects or elements of the initializer.

Like a reference, a structured binding is an alias to an existing object. Unlike
a reference, a structured binding does not have to be of a reference type.

```text
attr ﻿(optional) cv-auto ref-qualifier ﻿(optional) [ identifier-list ] = expression ;    (1)
attr ﻿(optional) cv-auto ref-qualifier ﻿(optional) [ identifier-list ]{ expression };    (2)
attr ﻿(optional) cv-auto ref-qualifier ﻿(optional) [ identifier-list ]( expression );    (3)
```

- **attr** — sequence of any number of attributes
- **cv-auto** — possibly cv-qualified type specifier `auto`, may also include
  storage-class-specifier `static` or `thread_local`; including `volatile` in
  cv-qualifiers is deprecated(since C++20)
- **ref-qualifier** — either `&` or `&&`
- **identifier-list** — list of comma-separated identifiers introduced by this
  declaration
- **expression** — an expression that does not have the comma operator at the
  top level (grammatically, an *assignment-expression*), and has either array or
  non-union class type. If expression refers to any of the names from
  identifier-list, the declaration is ill-formed.

A structured binding declaration introduces all identifiers in the
identifier-list as names in the surrounding scope and binds them to subobjects
or elements of the object denoted by expression. The bindings so introduced are
called *structured bindings*.

A structured binding declaration first introduces a uniquely-named variable
(here denoted by `e`) to hold the value of the initializer, as follows:

- If expression has array type `A` and no ref-qualifier is present, then `e` has
  type *cv* `A`, where *cv* is the cv-qualifiers in the cv-auto sequence, and
  each element of `e` is copy- (for (1)) or direct- (for (2,3)) initialized from
  the corresponding element of expression.
- Otherwise `e` is defined as if by using its name instead of `[`
  identifier-list `]` in the declaration.

We use `E` to denote the type of the expression `e`. (In other words, `E` is the
equivalent of `std::remove_reference_t<decltype((e))>`.)

A structured binding declaration then performs the binding in one of three
possible ways, depending on `E`:

- Case 1: if `E` is an array type, then the names are bound to the array
  elements.
- Case 2: if `E` is a non-union class type and `std::tuple_size<E>` is a
  complete type with a member named `value` (regardless of the type or
  accessibility of such member), then the "tuple-like" binding protocol is used.
- Case 3: if `E` is a non-union class type but `std::tuple_size<E>` is not a
  complete type, then the names are bound to the accessible data members of `E`.

Each of the three cases is described in more detail below.

Each structured binding has a *referenced type*, defined in the description
below. This type is the type returned by `decltype` when applied to an
unparenthesized structured binding.

#### Case 1: binding an array

Each identifier in the identifier-list becomes the name of an lvalue that refers
to the corresponding element of the array. The number of identifiers must equal
the number of array elements.

The *referenced type* for each identifier is the array element type. Note that
if the array type `E` is cv-qualified, so is its element type.

```cpp
int a[2] = {1, 2};

auto [x, y] = a;    // creates e[2], copies a into e,
                    // then x refers to e[0], y refers to e[1]
auto& [xr, yr] = a; // xr refers to a[0], yr refers to a[1]
```

#### Case 2: binding a tuple-like type

The expression `std::tuple_size<E>::value` must be a well-formed integer
constant expression, and the number of identifiers must equal
`std::tuple_size<E>::value`.

For each identifier, a variable whose type is "reference to
`std::tuple_element<i, E>::type`" is introduced: lvalue reference if its
corresponding initializer is an lvalue, rvalue reference otherwise. The
initializer for the i-th variable is

- `e.get<i>()`, if lookup for the identifier `get` in the scope of `E` by class
  member access lookup finds at least one declaration that is a function
  template whose first template parameter is a non-type parameter
- Otherwise, `get<i>(e)`, where `get` is looked up by argument-dependent lookup
  only, ignoring non-ADL lookup.

In these initializer expressions, `e` is an lvalue if the type of the entity `e`
is an lvalue reference (this only happens if the ref-qualifier is `&` or if it
is `&&` and the initializer expression is an lvalue) and an xvalue otherwise
(this effectively performs a kind of perfect forwarding), `i` is a `std::size_t`
prvalue, and `<i>` is always interpreted as a template parameter list.

The variable has the same storage duration as `e`.

The identifier then becomes the name of an lvalue that refers to the object
bound to said variable.

The *referenced type* for the i-th identifier is `std::tuple_element<i,
E>::type`.

```cpp
float x{};
char  y{};
int   z{};

std::tuple<float&, char&&, int> tpl(x, std::move(y), z);
const auto& [a, b, c] = tpl;
// using Tpl = const std::tuple<float&, char&&, int>;
// a names a structured binding that refers to x (initialized from get<0>(tpl))
// decltype(a) is std::tuple_element<0, Tpl>::type, i.e. float&
// b names a structured binding that refers to y (initialized from get<1>(tpl))
// decltype(b) is std::tuple_element<1, Tpl>::type, i.e. char&&
// c names a structured binding that refers to the third component of tpl, get<2>(tpl)
// decltype(c) is std::tuple_element<2, Tpl>::type, i.e. const int
```

#### Case 3: binding to data members

Every non-static data member of `E` must be a direct member of `E` or the same
base class of `E`, and must be well-formed in the context of the structured
binding when named as `e.name`. `E` may not have an anonymous union member. The
number of identifiers must equal the number of non-static data members.

Each identifier in identifier-list becomes the name of an lvalue that refers to
the next member of `e` in declaration order (bit-fields are supported); the type
of the lvalue is that of `e.m_i`, where `m_i` refers to the ith member.

The *referenced type* of the i-th identifier is the type of `e.m_i` if it is not
a reference type, or the declared type of `m_i` otherwise.

```cpp
#include <iostream>

struct S
{
    mutable int x1 : 2;
    volatile double y1;
};

S f() { return S{1, 2.3}; }

int main()
{
    const auto [x, y] = f(); // x is an int lvalue identifying the 2-bit bit-field
                             // y is a const volatile double lvalue
    std::cout << x << ' ' << y << '\n';  // 1 2.3
    x = -2;   // OK
//  y = -2.;  // Error: y is const-qualified
    std::cout << x << ' ' << y << '\n';  // -2 2.3
}
```

### Notes

Structured bindings cannot be constrained:
```cpp
template<class T>
concept C = true;
C auto [x, y] = std::pair{1, 2}; // error: constrained
```
*(since C++20)*

The lookup for member `get` ignores accessibility as usual and also ignores the
exact type of the non-type template parameter. A private `template<char*> void
get();` member will cause the member interpretation to be used, even though it
is ill-formed.

The portion of the declaration preceding `[` applies to the hidden variable `e`,
not to the introduced identifiers:

```cpp
int a = 1, b = 2;
const auto& [x, y] = std::tie(a, b); // x and y are of type int&
auto [z, w] = std::tie(a, b);        // z and w are still of type int&
assert(&z == &a);                    // passes
```

The tuple-like interpretation is always used if `std::tuple_size<E>` is a
complete type with a member named `value`, even if that would cause the program
to be ill-formed:

```cpp
struct A { int x; };

namespace std
{
    template<>
    struct tuple_size<::A> { void value(); };
}

auto [x] = A{}; // error; the "data member" interpretation is not considered.
```

The usual rules for reference-binding to temporaries (including
lifetime-extension) apply if a ref-qualifier is present and the expression is a
prvalue. In those cases the hidden variable `e` is a reference that binds to the
temporary variable materialized from the prvalue expression, extending its
lifetime. As usual, the binding will fail if `e` is a non-const lvalue
reference:

```cpp
int a = 1;

const auto& [x] = std::make_tuple(a); // OK, not dangling
auto&       [y] = std::make_tuple(a); // error, cannot bind auto& to rvalue std::tuple
auto&&      [z] = std::make_tuple(a); // also OK
```

`decltype(x)`, where `x` denotes a structured binding, names the *referenced
type* of that structured binding. In the tuple-like case, this is the type
returned by `std::tuple_element`, which may not be a reference even though a
hidden reference is always introduced in this case. This effectively emulates
the behavior of binding to a struct whose non-static data members have the types
returned by `tuple_element`, with the referenceness of the binding itself being
a mere implementation detail.

```cpp
std::tuple<int, int&> f();

auto [x, y] = f();       // decltype(x) is int
                         // decltype(y) is int&

const auto [z, w] = f(); // decltype(z) is const int
                         // decltype(w) is int&
```

Structured bindings cannot be captured by lambda expressions:
```cpp
#include <cassert>
int main()
{
    struct S { int p{6}, q{7}; };
    const auto& [b, d] = S{};
    auto l = [b, d] { return b * d; }; // valid since C++20
    assert(l() == 42);
}
```
*(until C++20)*

  Feature-test macro | Value | Std | Feature
  `__cpp_structured_bindings` | 201606L | (C++17) | Structured bindings

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <set>
#include <string>

int main()
{
    std::set<std::string> myset{"hello"};

    for (int i{2}; i; --i)
    {
        if (auto [iter, success] = myset.insert("Hello"); success)
            std::cout << "Insert is successful. The value is "
                      << std::quoted(*iter) << ".\n";
        else
            std::cout << "The value " << std::quoted(*iter)
                      << " already exists in the set.\n";
    }

    struct BitFields
    {
        // C++20: default member initializer for bit-fields
        int b : 4 {1}, d : 4 {2}, p : 4 {3}, q : 4 {4};
    };

    {
        const auto [b, d, p, q] = BitFields{};
        std::cout << b << ' ' << d << ' ' << p << ' ' << q << '\n';
    }

    {
        const auto [b, d, p, q] = []{ return BitFields{4, 3, 2, 1}; }();
        std::cout << b << ' ' << d << ' ' << p << ' ' << q << '\n';
    }

    {
        BitFields s;

        auto& [b, d, p, q] = s;
        std::cout << b << ' ' << d << ' ' << p << ' ' << q << '\n';

        b = 4, d = 3, p = 2, q = 1;
        std::cout << s.b << ' ' << s.d << ' ' << s.p << ' ' << s.q << '\n';
    }
}
```

Output:

```text
Insert is successful. The value is "Hello".
The value "Hello" already exists in the set.
1 2 3 4
4 3 2 1
1 2 3 4
4 3 2 1
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 2285 | C++17 | expression could refer to the names from identifier-list |
      the declaration is ill-formed in this case
  CWG 2312 | C++17 | the meaning of `mutable` was lost in the binding-to-members
      case | its meaning is still kept
  CWG 2386 | C++17 | the "tuple-like" binding protocol is used whenever
      `tuple_size<E>` is a complete type | used only when `tuple_size<E>` has a
      member `value`
  CWG 2635 | C++20 | structured bindings could be constrained | prohibited
  P0961R1 | C++17 | in the tuple-like case, member `get` is used if lookup finds
      a `get` of any kind | only if lookup finds a function template with a
      non-type parameter
  P0969R0 | C++17 | in the binding-to-members case, the members are required to
      be public | only required to be accessible in the context of the
      declaration

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):
- C++17 standard (ISO/IEC 14882:2017):

### See also

- **tie (C++11)** — creates a `tuple` of lvalue references or unpacks a tuple
  into individual objects (function template)

---
*Source: https://en.cppreference.com/w/cpp/language/structured_binding*
