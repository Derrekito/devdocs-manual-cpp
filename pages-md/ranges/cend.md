# std::ranges::cend

```cpp
inline namespace /* unspecified */ {
    inline constexpr /* unspecified */ cend = /* unspecified */;
}  // (since C++20) (customization point object)
Call signature
template< class T >
    requires /* see below */
constexpr /* see below */ auto cend( T&& t );  // (since C++20)
```

Returns a sentinel for the constant iterator(since C++23) indicating the end of
a const-qualified(until C++23) range.

Let `CT` be
- `const std::remove_reference_t<T>&` if the argument is an lvalue (i.e. `T` is
  an lvalue reference type),
- `const T` otherwise.
A call to `ranges::cend` is expression-equivalent to
`ranges::end(static_cast<CT&&>(t))`.
*(until C++23)*

If the argument is an lvalue or
`ranges::enable_borrowed_range<std::remove_cv_t<T>>` is `true`, then a call to
`ranges::cend` is expression-equivalent to:
- `std::const_sentinel<decltype(U)>(U)` for some expression `U` equivalent to
  `ranges::end(/*possibly-const-range*/(t))`.
In all other cases, a call to `ranges::cend` is ill-formed, which can result in
substitution failure when the call appears in the immediate context of a
template instantiation.
*(since C++23)*

If `ranges::cend(e)` is valid for an expression `e`, where `decltype((e))` is
`T`, then `CT` models `std::ranges::range`, and(until C++23)
`std::sentinel_for<S, I>` is `true` in all cases, where `S` is
`decltype(ranges::cend(e))`, and `I` is `decltype(ranges::cbegin(e))`.
Additionally, `S` models `constant-iterator` if it models
`input_iterator`.(since C++23)

### Customization point objects

The name `ranges::cend` denotes a *customization point object*, which is a const
function object of a literal `semiregular` class type. For exposition purposes,
the cv-unqualified version of its type is denoted as `__cend_fn`.

All instances of `__cend_fn` are equal. The effects of invoking different
instances of type `__cend_fn` on the same arguments are equivalent, regardless
of whether the expression denoting the instance is an lvalue or rvalue, and is
const-qualified or not (however, a volatile-qualified instance is not required
to be invocable). Thus, `ranges::cend` can be copied freely and its copies can
be used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `ranges::cend` above, `__cend_fn` models

- `std::invocable<__cend_fn, Args...>`,
- `std::invocable<const __cend_fn, Args...>`,
- `std::invocable<__cend_fn&, Args...>`, and
- `std::invocable<const __cend_fn&, Args...>`.

Otherwise, no function call operator of `__cend_fn` participates in overload
resolution.

### Example

```cpp
#include <algorithm>
#include <cassert>
#include <ranges>
#include <vector>

int main()
{
    std::vector vec{3, 1, 4};
    int arr[]{5, 10, 15};

    assert(std::ranges::find(vec, 5) == std::ranges::cend(vec));
    assert(std::ranges::find(arr, 5) != std::ranges::cend(arr));
}
```

### See also

- **ranges::end (C++20)** — returns a sentinel indicating the end of a range
  (customization point object)
- **endcend (C++11)(C++14)** — returns an iterator to the end of a container or
  array (function template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/cend*
