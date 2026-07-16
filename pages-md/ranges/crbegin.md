# std::ranges::crbegin

```cpp
inline namespace /* unspecified */ {
    inline constexpr /* unspecified */ crbegin = /* unspecified */;
}  // (since C++20) (customization point object)
Call signature
template< class T >
    requires /* see below */
constexpr /* see below */ auto crbegin( T&& t );  // (since C++20)
```

Returns an iterator to the first element of the const-qualified argument that is
treated as a reversed sequence.
*(until C++23)*

Returns a constant iterator to the first element of the argument that is treated
as a reversed sequence.
*(since C++23)*

Let `CT` be
- `const std::remove_reference_t<T>&` if the argument is an lvalue (i.e. `T` is
  an lvalue reference type),
- `const T` otherwise.
A call to `ranges::crbegin` is expression-equivalent to
`ranges::rbegin(static_cast<CT&&>(t))`.
*(until C++23)*

If the argument is an lvalue or
`ranges::enable_borrowed_range<std::remove_cv_t<T>>` is `true`, then a call to
`ranges::crbegin` is expression-equivalent to:
- `std::const_iterator<decltype(U)>(U)` for some expression `U` equivalent to
  `ranges::rbegin(/*possibly-const-range*/(t))`.
In all other cases, a call to `ranges::crbegin` is ill-formed, which can result
in substitution failure when the call appears in the immediate context of a
template instantiation.
*(since C++23)*

The return type models `std::input_or_output_iterator` and
`constant-iterator`(since C++23) in all cases.

### Customization point objects

The name `ranges::crbegin` denotes a *customization point object*, which is a
const function object of a literal `semiregular` class type. For exposition
purposes, the cv-unqualified version of its type is denoted as `__crbegin_fn`.

All instances of `__crbegin_fn` are equal. The effects of invoking different
instances of type `__crbegin_fn` on the same arguments are equivalent,
regardless of whether the expression denoting the instance is an lvalue or
rvalue, and is const-qualified or not (however, a volatile-qualified instance is
not required to be invocable). Thus, `ranges::crbegin` can be copied freely and
its copies can be used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `ranges::crbegin` above, `__crbegin_fn` models

- `std::invocable<__crbegin_fn, Args...>`,
- `std::invocable<const __crbegin_fn, Args...>`,
- `std::invocable<__crbegin_fn&, Args...>`, and
- `std::invocable<const __crbegin_fn&, Args...>`.

Otherwise, no function call operator of `__crbegin_fn` participates in overload
resolution.

### Example

```cpp
#include <cassert>
#include <iterator>
#include <span>
#include <vector>

int main()
{
    std::vector<int> v{3, 1, 4};
    auto vi = std::ranges::crbegin(v);
    assert(*vi == 4);
    ++vi; // OK, iterator object is mutable
    assert(*vi == 1);
    // *vi = 13; // Error: underlying element is read-only

    int a[]{-5, 10, 15};
    auto ai = std::ranges::crbegin(a);
    assert(*ai == 15);

    // auto x_x = std::ranges::crbegin(std::vector<int>{6, 6, 6});
    // ill-formed: the argument is an rvalue (see Notes ↑)

    auto si = std::ranges::crbegin(std::span{a}); // OK
    assert(*si == 15);
    static_assert
    (
        std::ranges::enable_borrowed_range<std::remove_cv_t<decltype(std::span{a})>>
    );
}
```

### See also

- **ranges::rbegin (C++20)** — returns a reverse iterator to a range
  (customization point object)
- **rbegincrbegin (C++14)** — returns a reverse iterator to the beginning of a
  container or array (function template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/crbegin*
