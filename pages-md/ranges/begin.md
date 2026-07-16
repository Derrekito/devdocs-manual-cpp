# std::ranges::begin

```cpp
inline namespace /* unspecified */ {
    inline constexpr /* unspecified */ begin = /* unspecified */;
}  // (since C++20) (customization point object)
Call signature
template< class T >
    requires /* see below */
constexpr std::input_or_output_iterator auto begin( T&& t );  // (since C++20)
```

Returns an iterator to the first element of the argument.

If the argument is an lvalue or
`ranges::enable_borrowed_range<std::remove_cv_t<T>>` is `true`, then a call to
`ranges::begin` is expression-equivalent to:

1. `t + 0` if `t` has an array type. If
   std::remove_all_extents_t<std::remove_reference_t<T>> is incomplete, then the
   call to `ranges::begin` is ill-formed, no diagnostic required.
1. Otherwise, `decay-copy(t.begin())`(until C++23)`auto(t.begin())`(since
   C++23), if that expression is valid and its type models
   `std::input_or_output_iterator`.
1. Otherwise, `decay-copy(begin(t))`(until C++23)`auto(begin(t))`(since C++23),
   if `T` is a class or enumeration type, that expression is valid and its type
   models `std::input_or_output_iterator`, where the meaning of `begin` is
   established as if by performing argument-dependent lookup only.

In all other cases, a call to `ranges::begin` is ill-formed, which can result in
substitution failure when the call appears in the immediate context of a
template instantiation.

### Customization point objects

The name `ranges::begin` denotes a *customization point object*, which is a
const function object of a literal `semiregular` class type. For exposition
purposes, the cv-unqualified version of its type is denoted as `__begin_fn`.

All instances of `__begin_fn` are equal. The effects of invoking different
instances of type `__begin_fn` on the same arguments are equivalent, regardless
of whether the expression denoting the instance is an lvalue or rvalue, and is
const-qualified or not (however, a volatile-qualified instance is not required
to be invocable). Thus, `ranges::begin` can be copied freely and its copies can
be used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `ranges::begin` above, `__begin_fn` models

- `std::invocable<__begin_fn, Args...>`,
- `std::invocable<const __begin_fn, Args...>`,
- `std::invocable<__begin_fn&, Args...>`, and
- `std::invocable<const __begin_fn&, Args...>`.

Otherwise, no function call operator of `__begin_fn` participates in overload
resolution.

### Notes

If the argument is an rvalue (i.e. `T` is an object type) and
`ranges::enable_borrowed_range<std::remove_cv_t<T>>` is `false`, the call to
`ranges::begin` is ill-formed, which also results in substitution failure.

The return type models `std::input_or_output_iterator` in all cases.

The C++20 standard requires that if the underlying `begin` function call returns
a prvalue, the return value is move-constructed from the materialized temporary
object. All implementations directly return the prvalue instead. The requirement
is corrected by the post-C++20 proposal P0849R8 to match the implementations.

### Example

```cpp
#include <cassert>
#include <ranges>
#include <vector>

int main()
{
    std::vector v{3, 1, 4};
    auto vi = std::ranges::begin(v);
    auto vci = std::ranges::cbegin(v);
    assert(*vi == 3 and *vi == *vci);
    ++vi;
    ++vci; // OK: vci is modifiable object
    *vi = 42; // OK: vi points to mutable element
    // *vci = 13; // Error: vci points to immutable element

    int a[]{-5, 10, 15};
    auto ai = std::ranges::begin(a); // works with C-arrays as well
    assert(*ai == -5);
    *ai = 42; // OK
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2602R2 | C++20 | there's machinery to prohibit certain non-member `begin`
      found by ADL | removed such machinery

### See also

- **ranges::cbegin (C++20)** — returns an iterator to the beginning of a
  read-only range (customization point object)
- **begincbegin (C++11)(C++14)** — returns an iterator to the beginning of a
  container or array (function template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/begin*
