# std::ranges::cdata

```cpp
inline namespace /*unspecified*/ {
    inline constexpr /*unspecified*/ cdata = /*unspecified*/;
}  // (since C++20) (customization point object)
Call signature
template< class T >
    requires /* see below */
constexpr /* see below */ cdata( T&& t );  // (since C++20)
```

Returns a pointer to the first element of constant type(since C++23) of a
contiguous range denoted by a const-qualified(until C++23) argument.

Let `CT` be
- `const std::remove_reference_t<T>&` if the argument is an lvalue (i.e. `T` is
  an lvalue reference type),
- `const T` otherwise.
A call to `ranges::cdata` is expression-equivalent to
`ranges::data(static_cast<CT&&>(t))`.
The return type is equivalent to
`std::remove_reference_t<ranges::range_reference_t<CT>>*`.
*(until C++23)*

If the argument is an lvalue or
`ranges::enable_borrowed_range<std::remove_cv_t<T>>` is `true`, then a call to
`ranges::cdata` is expression-equivalent to:
- `/*as-const-pointer*/(ranges::data(/*possibly-const-range*/(t)))`.
The return type is equivalent to
`std::remove_reference_t<ranges::range_const_reference_t<T>>*`.
In all other cases, a call to `ranges::cdata` is ill-formed, which can result in
substitution failure when the call appears in the immediate context of a
template instantiation.
*(since C++23)*

If `ranges::cdata(t)` is valid, then it returns a pointer to an object of
constant type(since C++23).

### Customization point objects

The name `ranges::cdata` denotes a *customization point object*, which is a
const function object of a literal `semiregular` class type. For exposition
purposes, the cv-unqualified version of its type is denoted as `__cdata_fn`.

All instances of `__cdata_fn` are equal. The effects of invoking different
instances of type `__cdata_fn` on the same arguments are equivalent, regardless
of whether the expression denoting the instance is an lvalue or rvalue, and is
const-qualified or not (however, a volatile-qualified instance is not required
to be invocable). Thus, `ranges::cdata` can be copied freely and its copies can
be used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `ranges::cdata` above, `__cdata_fn` models

- `std::invocable<__cdata_fn, Args...>`,
- `std::invocable<const __cdata_fn, Args...>`,
- `std::invocable<__cdata_fn&, Args...>`, and
- `std::invocable<const __cdata_fn&, Args...>`.

Otherwise, no function call operator of `__cdata_fn` participates in overload
resolution.

### Example

```cpp
#include <cstring>
#include <iostream>
#include <ranges>
#include <string>

int main()
{
    std::string src {"hello world!\n"};

//  std::ranges::cdata(src)[0] = 'H'; // error, src.data() is treated as read-only
    std::ranges::data(src)[0] = 'H'; // OK, src.data() is a non-const storage

    char dst[20]; // storage for a C-style string
    std::strcpy(dst, std::ranges::cdata(src));
    // [data(src), data(src) + size(src)] is guaranteed to be an NTBS

    std::cout << dst;
}
```

Output:

```text
Hello world!
```

### See also

- **ranges::data (C++20)** — obtains a pointer to the beginning of a contiguous
  range (customization point object)
- **data (C++17)** — obtains the pointer to the underlying array (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/cdata*
