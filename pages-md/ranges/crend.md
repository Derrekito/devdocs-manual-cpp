# std::ranges::crend

```cpp
inline namespace /* unspecified */ {
    inline constexpr /* unspecified */ crend = /* unspecified */;
}  // (since C++20) (customization point object)
Call signature
template< class T >
    requires /* see below */
constexpr /* see below */ auto crend( T&& t );  // (since C++20)
```

Returns a sentinel for the constant iterator(since C++23) indicating the end of
a const-qualified(until C++23) range that is treated as a reversed sequence.

Let `CT` be
- `const std::remove_reference_t<T>&` if the argument is an lvalue (i.e. `T` is
  an lvalue reference type),
- `const T` otherwise.
A call to `ranges::crend` is expression-equivalent to
`ranges::rend(static_cast<CT&&>(t))`.
*(until C++23)*

If the argument is an lvalue or
`ranges::enable_borrowed_range<std::remove_cv_t<T>>` is `true`, then a call to
`ranges::crend` is expression-equivalent to:
- `std::const_sentinel<decltype(U)>(U)` for some expression `U` equivalent to
  `ranges::rend(/*possibly-const-range*/(t))`.
In all other cases, a call to `ranges::crend` is ill-formed, which can result in
substitution failure when the call appears in the immediate context of a
template instantiation.
*(since C++23)*

If `ranges::crend(e)` is valid for an expression `e`, where `decltype((e))` is
`T`, then `CT` models `std::ranges::range`, and(until C++23)
`std::sentinel_for<S, I>` is `true` in all cases, where `S` is
`decltype(ranges::crend(e))`, and `I` is `decltype(ranges::crbegin(e))`.
Additionally, `S` models `constant-iterator` if it models
`input_iterator`.(since C++23)

### Customization point objects

The name `ranges::crend` denotes a *customization point object*, which is a
const function object of a literal `semiregular` class type. For exposition
purposes, the cv-unqualified version of its type is denoted as `__crend_fn`.

All instances of `__crend_fn` are equal. The effects of invoking different
instances of type `__crend_fn` on the same arguments are equivalent, regardless
of whether the expression denoting the instance is an lvalue or rvalue, and is
const-qualified or not (however, a volatile-qualified instance is not required
to be invocable). Thus, `ranges::crend` can be copied freely and its copies can
be used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `ranges::crend` above, `__crend_fn` models

- `std::invocable<__crend_fn, Args...>`,
- `std::invocable<const __crend_fn, Args...>`,
- `std::invocable<__crend_fn&, Args...>`, and
- `std::invocable<const __crend_fn&, Args...>`.

Otherwise, no function call operator of `__crend_fn` participates in overload
resolution.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    int a[]{4, 6, -3, 9, 10};
    std::cout << "Array backwards: ";
    namespace ranges = std::ranges;
    ranges::copy(ranges::rbegin(a), ranges::rend(a),
                 std::ostream_iterator<int>(std::cout, " "));
    std::cout << '\n';

    std::cout << "Vector backwards: ";
    std::vector v{4, 6, -3, 9, 10};
    ranges::copy(ranges::rbegin(v), ranges::rend(v),
                 std::ostream_iterator<int>(std::cout, " "));
    std::cout << '\n';
}
```

Output:

```text
Array backwards: 10 9 -3 6 4
Vector backwards: 10 9 -3 6 4
```

### See also

- **ranges::rend (C++20)** — returns a reverse end iterator to a range
  (customization point object)
- **rendcrend (C++14)** — returns a reverse end iterator for a container or
  array (function template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/crend*
