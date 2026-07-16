# std::ranges::views::all, std::ranges::views::all_t

```cpp
inline constexpr /* unspecified */ all = /* unspecified */;  // (1) (since C++20)
template< ranges::viewable_range R >
using all_t = decltype(views::all(std::declval<R>()));  // (2) (since C++20)
```

1) A RangeAdaptorObject (also a RangeAdaptorClosureObject) that returns a `view`
   that includes all elements of its `range` argument.
Given an expression `e` of type `R`, the expression `views::all(e)` is
expression-equivalent to:

- Implicitly converting `e` to a std::decay_t<R> prvalue, if std::decay_t<R>
  models `view`.
- Otherwise, `std::ranges::ref_view{e}` if that expression is well-formed.
- Otherwise, `std::ranges::owning_view{e}`.

2) Calculates the suitable `view` type of a `viewable_range` type.

### Example

```cpp
#include <iostream>
#include <ranges>
#include <type_traits>
#include <vector>

int main()
{
    std::vector<int> v{0, 1, 2, 3, 4, 5};
    for (int n : std::views::all(v) | std::views::take(2))
        std::cout << n << ' ';
    std::cout << '\n';

    static_assert(std::is_same<
        decltype(std::views::single(42)),
        std::ranges::single_view<int>
        >{});

    static_assert(std::is_same<
        decltype(std::views::all(v)),
        std::ranges::ref_view<std::vector<int, std::allocator<int>>>
        >{});

    int a[]{1, 2, 3, 4};
    static_assert(std::is_same<
        decltype(std::views::all(a)),
        std::ranges::ref_view<int[4]>
        >{});

    static_assert(std::is_same<
        decltype(std::ranges::subrange{std::begin(a) + 1, std::end(a) - 1}),
        std::ranges::subrange<int*, int*, std::ranges::subrange_kind(1)>
        >{});
}
```

Output:

```text
0 1
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3724 | C++20 | `views::all` might result in hard error for some move-only
      views | it gets well-constrained
  P2415R2 | C++20 | `views::all` returned a `subrange` for a non-`view` rvalue
      `range` | returns an `owning_view` for it

### See also

- **ranges::empty_viewviews::empty (C++20)** — an empty `view` with no elements
  (class template) (variable template)
- **ranges::single_viewviews::single (C++20)** — a `view` that contains a single
  element of a specified value (class template) (customization point object)
- **ranges::owning_view (C++20)** — a `view` with unique ownership of some
  `range` (class template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/all_view*
