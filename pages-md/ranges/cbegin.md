# std::ranges::cbegin

```cpp
inline namespace /* unspecified */ {
    inline constexpr /* unspecified */ cbegin = /* unspecified */;
}  // (since C++20) (customization point object)
Call signature
template< class T >
    requires /* see below */
constexpr /* see below */ auto cbegin( T&& t );  // (since C++20)
```

Returns an iterator to the first element of the const-qualified argument.
*(until C++23)*

Returns a constant iterator to the first element of the argument.
*(since C++23)*

Let `CT` be
- `const std::remove_reference_t<T>&` if the argument is an lvalue (i.e. `T` is
  an lvalue reference type),
- `const T` otherwise.
A call to `ranges::cbegin` is expression-equivalent to
`ranges::begin(static_cast<CT&&>(t))`.
*(until C++23)*

If the argument is an lvalue or
`ranges::enable_borrowed_range<std::remove_cv_t<T>>` is `true`, then a call to
`ranges::cbegin` is expression-equivalent to:
- `std::const_iterator<decltype(U)>(U)` for some expression `U` equivalent to
  `ranges::begin(/*possibly-const-range*/(t))`.
In all other cases, a call to `ranges::cbegin` is ill-formed, which can result
in substitution failure when the call appears in the immediate context of a
template instantiation.
*(since C++23)*

The return type models `std::input_or_output_iterator` and
`constant-iterator`(since C++23) in all cases.

### Customization point objects

The name `ranges::cbegin` denotes a *customization point object*, which is a
const function object of a literal `semiregular` class type. For exposition
purposes, the cv-unqualified version of its type is denoted as `__cbegin_fn`.

All instances of `__cbegin_fn` are equal. The effects of invoking different
instances of type `__cbegin_fn` on the same arguments are equivalent, regardless
of whether the expression denoting the instance is an lvalue or rvalue, and is
const-qualified or not (however, a volatile-qualified instance is not required
to be invocable). Thus, `ranges::cbegin` can be copied freely and its copies can
be used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `ranges::cbegin` above, `__cbegin_fn` models

- `std::invocable<__cbegin_fn, Args...>`,
- `std::invocable<const __cbegin_fn, Args...>`,
- `std::invocable<__cbegin_fn&, Args...>`, and
- `std::invocable<const __cbegin_fn&, Args...>`.

Otherwise, no function call operator of `__cbegin_fn` participates in overload
resolution.

### Notes

For an lvalue range `e` of type `T`, `ranges::cbegin(e)` is equivalent to

`ranges::begin(std::as_const(e))`.
*(until C++23)*

- `ranges::begin(e)` if `T` models `constant_range`.
- Otherwise, `ranges::begin(std::as_const(e))` if `const T` models
  `constant_range`.
- Otherwise, `std::basic_const_iterator(ranges::begin(e))`.
*(since C++23)*

### Example

```cpp
#include <cassert>
#include <ranges>
#include <vector>

int main()
{
    std::vector v{3, 1, 4};
    auto vi = std::ranges::cbegin(v);
    assert(3 == *vi);
    ++vi; // OK, constant-iterator object is mutable
    assert(1 == *vi);
    // *vi = 13; // Error: constant-iterator points to an immutable element

    int a[]{3, 1, 4};
    auto ai = std::ranges::cbegin(a); // cbegin works with C-arrays as well
    assert(3 == *ai and *(ai + 1) == 1);
    // *ai = 13; // Error: read-only variable is not assignable
}
```

### See also

- **ranges::begin (C++20)** — returns an iterator to the beginning of a range
  (customization point object)
- **begincbegin (C++11)(C++14)** — returns an iterator to the beginning of a
  container or array (function template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/cbegin*
