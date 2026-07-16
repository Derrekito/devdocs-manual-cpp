# std::ranges::size

```cpp
inline namespace /* unspecified */ {
    inline constexpr auto size = /* unspecified */;
}  // (since C++20) (customization point object)
Call signature
template< class T >
    requires /* see below */
constexpr auto size( T&& t );  // (since C++20)
```

Calculates the number of elements in `t` in constant time.

A call to `ranges::size` is expression-equivalent to:

1. `decay-copy(std::extent_v<T>)`(until C++23)`auto(std::extent_v<T>)`(since
   C++23), if `T` is an array type with a known bound.
1. Otherwise, `decay-copy(t.size())`(until C++23)`auto(t.size())`(since C++23),
   if `ranges::disable_sized_range<std::remove_cv_t<T>>` is `false`, and that
   expression is valid and has an integer-like type.
1. Otherwise, `decay-copy(size(t))`(until C++23)`auto(size(t))`(since C++23), if
   `ranges::disable_sized_range<std::remove_cv_t<T>>` is `false`, and the
   converted expression is valid and has an integer-like type, where the meaning
   of `size` is established as if by performing argument-dependent lookup only.
1. Otherwise, `/* to-unsigned-like */(ranges::end(t) - ranges::begin(t))`, if
   `T` models `ranges::forward_range` and `ranges::sentinel_t<T>` models
   `std::sized_sentinel_for<ranges::iterator_t<T>>`, where `/* to-unsigned-like
   */` denotes an explicit conversion to an unsigned-integer-like type.

In all other cases, a call to `ranges::size` is ill-formed, which can result in
substitution failure when `ranges::size(t)` appears in the immediate context of
a template instantiation.

### Customization point objects

The name `ranges::size` denotes a *customization point object*, which is a const
function object of a literal `semiregular` class type. For exposition purposes,
the cv-unqualified version of its type is denoted as `__size_fn`.

All instances of `__size_fn` are equal. The effects of invoking different
instances of type `__size_fn` on the same arguments are equivalent, regardless
of whether the expression denoting the instance is an lvalue or rvalue, and is
const-qualified or not (however, a volatile-qualified instance is not required
to be invocable). Thus, `ranges::size` can be copied freely and its copies can
be used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `ranges::size` above, `__size_fn` models

- `std::invocable<__size_fn, Args...>`,
- `std::invocable<const __size_fn, Args...>`,
- `std::invocable<__size_fn&, Args...>`, and
- `std::invocable<const __size_fn&, Args...>`.

Otherwise, no function call operator of `__size_fn` participates in overload
resolution.

### Notes

Whenever `ranges::size(e)` is valid for an expression `e`, the return type is
integer-like.

The C++20 standard requires that if the underlying `size` function call returns
a prvalue, the return value is move-constructed from the materialized temporary
object. All implementations directly return the prvalue instead. The requirement
is corrected by the post-C++20 proposal P0849R8 to match the implementations.

The expression `ranges::distance(e)` can also be used to determine the size of a
range `e`. Unlike `ranges::size(e)`, `ranges::distance(e)` works even if `e` is
an unsized range, at the cost of having linear complexity in that case.

### Example

```cpp
#include <iostream>
#include <ranges>
#include <type_traits>
#include <vector>

int main()
{
    auto v = std::vector<int>{};
    std::cout << "ranges::size(v) == " << std::ranges::size(v) << '\n';

    auto il = {7};     // std::initializer_list
    std::cout << "ranges::size(il) == " << std::ranges::size(il) << '\n';

    int array[]{4, 5}; // array has a known bound
    std::cout << "ranges::size(array) == " << std::ranges::size(array) << '\n';

    static_assert(std::is_signed_v<decltype(std::ranges::size(v))> == false);
}
```

Output:

```text
ranges::size(v) == 0
ranges::size(il) == 1
ranges::size(array) == 2
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2602R2 | C++20 | there's machinery to prohibit certain non-member `size`
      found by ADL | removed such machinery

### See also

- **ranges::ssize (C++20)** — returns a signed integer equal to the size of a
  range (customization point object)
- **ranges::sized_range (C++20)** — specifies that a range knows its size in
  constant time (concept)
- **ranges::distance (C++20)** — returns the distance between an iterator and a
  sentinel, or between the beginning and end of a range (niebloid)
- **sizessize (C++17)(C++20)** — returns the size of a container or array
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/size*
