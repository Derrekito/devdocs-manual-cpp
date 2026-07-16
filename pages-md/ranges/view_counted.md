# std::ranges::views::counted

```cpp
inline constexpr /*unspecified*/ counted = /*unspecified*/;  // (since C++20)
Call signature
template< class Iterator, class DifferenceType >
    requires /* see below */
constexpr /*span-or-subrange*/ counted( Iterator&& it, DifferenceType&& count );  // (since C++20)
```

A counted view presents a `view` of the elements of the *counted range*
`[`i`,`n`)` for some iterator `i` and non-negative integer `n`.

A counted range `[`i`,`n`)` is the `n` elements starting with the element
pointed to by `i` and up to but not including the element, if any, pointed to by
the result of `n` applications of `++i`.

If `n == 0`, the counted range is valid and empty. Otherwise, the counted range
is only valid if `n` is positive, `i` is dereferenceable, and
`[``++i``,``--n``)` is a valid counted range.

Formally, if `it` and `count` are expressions, `T` is
`std::decay_t<decltype((it))>`, and `D` is `std::iter_difference_t<T>`, then

  if `T` models `input_or_output_iterator` and `decltype((count))` models
  `std::convertible_to<D>`,

  - if `T` models `contiguous_iterator`, then `views::counted(it, count)` is
    expression-equivalent to `std::span(std::to_address(it),
    static_cast<std::size_t>(static_cast<D>(count)))`,
  - otherwise, if `T` models `random_access_iterator`, then `views::counted(it,
    count)` is expression-equivalent to `ranges::subrange(it, it +
    static_cast<D>(count))`,
  - otherwise, `views::counted(it, count)` is expression-equivalent to
    `ranges::subrange(std::counted_iterator(it, count), std::default_sentinel)`.

  Otherwise, `views::counted(it, count)` is ill-formed.

### Customization point objects

The name `views::counted` denotes a *customization point object*, which is a
const function object of a literal `semiregular` class type. For exposition
purposes, the cv-unqualified version of its type is denoted as `__counted_fn`.

All instances of `__counted_fn` are equal. The effects of invoking different
instances of type `__counted_fn` on the same arguments are equivalent,
regardless of whether the expression denoting the instance is an lvalue or
rvalue, and is const-qualified or not (however, a volatile-qualified instance is
not required to be invocable). Thus, `views::counted` can be copied freely and
its copies can be used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `views::counted` above, `__counted_fn` models

- `std::invocable<__counted_fn, Args...>`,
- `std::invocable<const __counted_fn, Args...>`,
- `std::invocable<__counted_fn&, Args...>`, and
- `std::invocable<const __counted_fn&, Args...>`.

Otherwise, no function call operator of `__counted_fn` participates in overload
resolution.

### Notes

`views::counted` does not check if the range is long enough to provide all
`count` elements: use views::take if that check is necessary.

### Example

```cpp
#include <iostream>
#include <ranges>

int main()
{
    const int a[]{1, 2, 3, 4, 5, 6, 7};
    for (int i : std::views::counted(a, 3))
        std::cout << i << ' ';
    std::cout << '\n';

    const auto il = {1, 2, 3, 4, 5};
    for (int i : std::views::counted(il.begin() + 1, 3))
        std::cout << i << ' ';
    std::cout << '\n';
}
```

Output:

```text
1 2 3
2 3 4
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2393R1 | C++20 | implicit conversion from an integer-class type to
      std::size_t might be invalid | made explicit

### See also

- **ranges::take_viewviews::take (C++20)** — a `view` consisting of the first N
  elements of another `view` (class template) (range adaptor object)
- **ranges::subrange (C++20)** — combines an iterator-sentinel pair into a
  `view` (class template)
- **counted_iterator (C++20)** — iterator adaptor that tracks the distance to
  the end of the range (class template)
- **ranges::countranges::count_if (C++20)(C++20)** — returns the number of
  elements satisfying specific criteria (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/ranges/view_counted*
