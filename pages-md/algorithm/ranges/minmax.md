# std::ranges::minmax, std::ranges::minmax_result

```cpp
Call signature
template< class T, class Proj = std::identity,
          std::indirect_strict_weak_order<
              std::projected<const T*, Proj>> Comp = ranges::less >
constexpr ranges::minmax_result<const T&>
    minmax( const T& a, const T& b, Comp comp = {}, Proj proj = {} );  // (1) (since C++20)
template< std::copyable T, class Proj = std::identity,
          std::indirect_strict_weak_order<
              std::projected<const T*, Proj>> Comp = ranges::less >
constexpr ranges::minmax_result<T>
    minmax( std::initializer_list<T> r, Comp comp = {}, Proj proj = {} );  // (2) (since C++20)
template< ranges::input_range R, class Proj = std::identity,
          std::indirect_strict_weak_order<
              std::projected<ranges::iterator_t<R>, Proj>> Comp = ranges::less >
requires std::indirectly_copyable_storable<ranges::iterator_t<R>, ranges::range_value_t<R>*>
constexpr ranges::minmax_result<ranges::range_value_t<R>>
    minmax( R&& r, Comp comp = {}, Proj proj = {} );  // (3) (since C++20)
Helper types
template< class T >
using minmax_result = ranges::min_max_result<T>;  // (4) (since C++20)
```

Returns the smallest and the greatest of the given projected values.

1) Returns references to the smaller and the greater of `a` and `b`.

2) Returns the smallest and the greatest of the values in the initializer list
   `r`.

3) Returns the smallest and the greatest of the values in the range `r`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **a, b** — the values to compare
- **r** — a non-empty range of values to compare
- **comp** — comparison to apply to the projected elements
- **proj** — projection to apply to the elements

### Return value

1) `{b, a}` if, according to their respective projected value, `b` is smaller
   than `a`; otherwise it returns `{a, b}`.

2,3) `{s, l}`, where `s` and `l` are respectively the smallest and largest
   values in `r`, according to their projected value. If several values are
   equivalent to the smallest and largest, returns the leftmost smallest value,
   and the rightmost largest value. If the range is empty (as determined by
   `ranges::distance(r)`), the behavior is undefined.

### Complexity

1) Exactly one comparison and two applications of the projection.

2,3) At most `3 / 2 * ranges::distance(r)` comparisons and twice as many
   applications of the projection.

### Possible implementation

```cpp
struct minmax_fn
{
    template<class T, class Proj = std::identity,
             std::indirect_strict_weak_order<
                 std::projected<const T*, Proj>> Comp = ranges::less>
    constexpr ranges::minmax_result<const T&>
         operator()(const T& a, const T& b, Comp comp = {}, Proj proj = {}) const
    {
        if (std::invoke(comp, std::invoke(proj, b), std::invoke(proj, a)))
            return {b, a};

        return {a, b};
    }

    template<std::copyable T, class Proj = std::identity,
             std::indirect_strict_weak_order<
                 std::projected<const T*, Proj>> Comp = ranges::less>
    constexpr ranges::minmax_result<T>
        operator()(std::initializer_list<T> r, Comp comp = {}, Proj proj = {}) const
    {
        auto result = ranges::minmax_element(r, std::ref(comp), std::ref(proj));
        return {*result.min, *result.max};
    }

    template<ranges::input_range R, class Proj = std::identity,
             std::indirect_strict_weak_order<
                 std::projected<ranges::iterator_t<R>, Proj>> Comp = ranges::less>
    requires std::indirectly_copyable_storable<ranges::iterator_t<R>,
                                               ranges::range_value_t<R>*>
    constexpr ranges::minmax_result<ranges::range_value_t<R>>
        operator()(R&& r, Comp comp = {}, Proj proj = {}) const
    {
        auto result = ranges::minmax_element(r, std::ref(comp), std::ref(proj));
        return {std::move(*result.min), std::move(*result.max)};
    }
};

inline constexpr minmax_fn minmax;
```

### Notes

For overload (1), if one of the parameters is a temporary, the reference
returned becomes a dangling reference at the end of the full expression that
contains the call to `minmax`:

```cpp
int n = 1;
auto p = std::ranges::minmax(n, n + 1);
int m = p.min; // ok
int x = p.max; // undefined behavior

// Note that structured bindings have the same issue
auto [mm, xx] = std::ranges::minmax(n, n + 1);
xx; // undefined behavior
```

### Example

```cpp
#include <algorithm>
#include <array>
#include <iostream>
#include <random>

int main()
{
    namespace ranges = std::ranges;

    constexpr std::array v{3, 1, 4, 1, 5, 9, 2, 6, 5};

    std::random_device rd;
    std::mt19937_64 generator(rd());
    std::uniform_int_distribution<> distribution(0, ranges::distance(v)); // [0..9]

    // auto bounds = ranges::minmax(distribution(generator), distribution(generator));
    // UB: dangling references: bounds.min and bounds.max have the type `const int&`.

    const int x1 = distribution(generator);
    const int x2 = distribution(generator);
    auto bounds = ranges::minmax(x1, x2); // OK: got references to lvalues x1 and x2

    std::cout << "v[" << bounds.min << ":" << bounds.max << "]: ";
    for (int i = bounds.min; i < bounds.max; ++i)
        std::cout << v[i] << ' ';
    std::cout << '\n';

    auto [min, max] = ranges::minmax(v);
    std::cout << "smallest: " << min << ", " << "largest: " << max << '\n';
}
```

Possible output:

```text
v[3:9]: 1 5 9 2 6 5
smallest: 1, largest: 9
```

### See also

- **ranges::min (C++20)** — returns the smaller of the given values (niebloid)
- **ranges::max (C++20)** — returns the greater of the given values (niebloid)
- **ranges::minmax_element (C++20)** — returns the smallest and the largest
  elements in a range (niebloid)
- **ranges::clamp (C++20)** — clamps a value between a pair of boundary values
  (niebloid)
- **minmax (C++11)** — returns the smaller and larger of two elements (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/minmax*
