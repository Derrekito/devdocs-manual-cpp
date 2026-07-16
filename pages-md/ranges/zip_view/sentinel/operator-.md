# operator-(ranges::zip_view::*sentinel*)

```cpp
template< bool OtherConst >
  requires (std::sized_sentinel_for<
              ranges::sentinel_t</*maybe-const*/<Const, Views>>,
              ranges::iterator_t</*maybe-const*/<OtherConst, Views>>> && ...)
friend constexpr
  std::common_type_t<ranges::range_difference_t</*maybe-const*/<OtherConst, Views>>...>
      operator-( const iterator<OtherConst>& x, const sentinel& y );  // (1) (since C++23)
template< bool OtherConst >
  requires (std::sized_sentinel_for<
              ranges::sentinel_t</*maybe-const*/<Const, Views>>,
              ranges::iterator_t</*maybe-const*/<OtherConst, Views>>> && ...)
friend constexpr
  std::common_type_t<ranges::range_difference_t</*maybe-const*/<OtherConst, Views>>...>
    operator-( const sentinel& y, const iterator<OtherConst>& x );  // (2) (since C++23)
```

Computes the minimal distance between the underlying tuple of iterators of `x`
and the underlying tuple of sentinels of `y`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `zip_view::sentinel<Const>`
is an associated class of the arguments.

### Parameters

- **x** — an iterator
- **y** — a sentinel

### Return value

Let `current_` denote the underlying tuple of iterators of `x`, and `end_`
denote the underlying tuple of sentinels of `y`.

Let `DIST(x, y, i)` be a distance calculated by expression equivalent to
`std::get<i>(x.current_) - std::get<i>(y.end_)` for some integer `i`.

1) the value with the smallest absolute value among `DIST(x, y, i)` of all `i`
   in range `0 ≤ i < sizeof...(Views)`

2) `-(x - y)`.

### Example

```cpp
#include <cassert>
#include <ranges>
#include <vector>
#include <deque>
#include <list>

int main() {
    auto x = std::vector{1, 2, 3, 4};
    auto y = std::deque{'a', 'b', 'c'};
    auto z = {1.1, 2.2};
    auto w = std::list{1, 2, 3};

    auto p = std::views::zip(x, y, z);
    assert(p.begin() - p.end() == +2);
    assert(p.end() - p.begin() == -2);

    [[maybe_unused]]
    auto q = std::views::zip(x, y, w);

    // The following code fires a compile-time error because std::list::iterator
    // does not support operator- that is needed to calculate the distance:
    // auto e = q.begin() - q.end();
}
```

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_view/sentinel/operator-*
