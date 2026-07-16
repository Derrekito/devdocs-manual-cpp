# std::ranges::ssize

```cpp
inline namespace /*unspecified*/ {
    inline constexpr /*unspecified*/ ssize = /*unspecified*/;
}  // (since C++20) (customization point object)
Call signature
template< class T >
    requires /* see below */
constexpr /*signed-integer-like*/ ssize( T&& t );  // (since C++20)
```

Returns the size of a range converted to a signed type.

If `ranges::size(t)` is well-formed, a call to `ranges::ssize` is
expression-equivalent to `static_cast<MadeSigned>(ranges::size(t))`, where
`MadeSigned` denotes

- the corresponding signed version of `decltype(ranges::size(t))`, if it is
  wider than `std::ptrdiff_t`, or
- `std::ptrdiff_t` otherwise.

If `ranges::size(t)` is ill-formed, a call to `ranges::ssize` is also
ill-formed, which can result in substitution failure when `ranges::ssize(t)`
appears in the immediate context of a template instantiation.

### Customization point objects

The name `ranges::ssize` denotes a *customization point object*, which is a
const function object of a literal `semiregular` class type. For exposition
purposes, the cv-unqualified version of its type is denoted as `__ssize_fn`.

All instances of `__ssize_fn` are equal. The effects of invoking different
instances of type `__ssize_fn` on the same arguments are equivalent, regardless
of whether the expression denoting the instance is an lvalue or rvalue, and is
const-qualified or not (however, a volatile-qualified instance is not required
to be invocable). Thus, `ranges::ssize` can be copied freely and its copies can
be used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `ranges::ssize` above, `__ssize_fn` models

- `std::invocable<__ssize_fn, Args...>`,
- `std::invocable<const __ssize_fn, Args...>`,
- `std::invocable<__ssize_fn&, Args...>`, and
- `std::invocable<const __ssize_fn&, Args...>`.

Otherwise, no function call operator of `__ssize_fn` participates in overload
resolution.

### Notes

If `ranges::ssize(e)` is valid for an expression `e`, the return type is a
signed-integer-like type, i.e. an integer type for which `std::is_signed_v` is
`true`, or a signed-integer-class type.

The width of integer-like types can be detected by
`std::numeric_limits::digits`.

### Example

```cpp
#include <array>
#include <iostream>
#include <ranges>
#include <type_traits>

int main()
{
    std::array arr{1, 2, 3, 4, 5};
    auto s = std::ranges::ssize(arr);

    std::cout << "ranges::ssize(arr) = " << s << '\n'
              << "ranges::ssize is "
              << (std::is_signed_v<decltype(s)> ? "signed" : "unsigned")
              << '\n';

    std::cout << "reversed arr: ";

    for (--s; s >= 0; --s)
        std::cout << arr[s] << ' ';

    std::cout << "\n" "s = " << s << '\n';
}
```

Output:

```text
ranges::ssize(arr) = 5
ranges::ssize is signed
reversed arr: 5 4 3 2 1
s = -1
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3403 | C++20 | `ranges::size` worked for some non-range types but
      `ranges::ssize` didn't | made work

### See also

- **ranges::size (C++20)** — returns an integer equal to the size of a range
  (customization point object)
- **ranges::sized_range (C++20)** — specifies that a range knows its size in
  constant time (concept)
- **ranges::distance (C++20)** — returns the distance between an iterator and a
  sentinel, or between the beginning and end of a range (niebloid)
- **sizessize (C++17)(C++20)** — returns the size of a container or array
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/ssize*
