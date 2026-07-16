# std::ranges::max_element

```cpp
Call signature
template< std::forward_iterator I, std::sentinel_for<I> S, class Proj = std::identity,
          std::indirect_strict_weak_order<std::projected<I, Proj>> Comp = ranges::less >
constexpr I
    max_element( I first, S last, Comp comp = {}, Proj proj = {} );  // (1) (since C++20)
template< ranges::forward_range R, class Proj = std::identity,
          std::indirect_strict_weak_order<
              std::projected<ranges::iterator_t<R>, Proj>> Comp = ranges::less >
constexpr ranges::borrowed_iterator_t<R>
    max_element( R&& r, Comp comp = {}, Proj proj = {} );  // (2) (since C++20)
```

1) Finds the greatest element in the range `[``first``,``last``)`.

2) Same as (1), but uses `r` as the source range, as if using `ranges::begin(r)`
   as `first` and `ranges::end(r)` as `last`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first, last** — iterator-sentinel pair denoting the range to examine
- **r** — the range to examine
- **comp** — comparison to apply to the projected elements
- **proj** — projection to apply to the elements

### Return value

Iterator to the greatest element in the range `[``first``,``last``)`. If several
elements in the range are equivalent to the greatest element, returns the
iterator to the first such element. Returns `first` if the range is empty.

### Complexity

Exactly max(N - 1, 0) comparisons, where `N = ranges::distance(first, last)`.

### Possible implementation

```cpp
struct max_element_fn
{
    template<std::forward_iterator I, std::sentinel_for<I> S, class Proj = std::identity,
             std::indirect_strict_weak_order<std::projected<I, Proj>> Comp = ranges::less>
    constexpr I operator()(I first, S last, Comp comp = {}, Proj proj = {}) const
    {
        if (first == last)
            return last;

        auto largest = first;
        ++first;
        for (; first != last; ++first)
            if (std::invoke(comp, std::invoke(proj, *largest), std::invoke(proj, *first)))
                largest = first;
        return largest;
    }

    template<ranges::forward_range R, class Proj = std::identity,
             std::indirect_strict_weak_order<
                 std::projected<ranges::iterator_t<R>, Proj>> Comp = ranges::less>
    constexpr ranges::borrowed_iterator_t<R>
        operator()(R&& r, Comp comp = {}, Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), std::ref(comp), std::ref(proj));
    }
};

inline constexpr max_element_fn max_element;
```

### Example

```cpp
#include <algorithm>
#include <cmath>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v {3, 1, -14, 1, 5, 9};

    namespace ranges = std::ranges;
    auto result = ranges::max_element(v.begin(), v.end());
    std::cout << "Max element at pos " << ranges::distance(v.begin(), result) << '\n';

    auto abs_compare = [](int a, int b) { return std::abs(a) < std::abs(b); };
    result = ranges::max_element(v, abs_compare);
    std::cout << "Absolute max element at pos "
              << ranges::distance(v.begin(), result) << '\n';
}
```

Output:

```text
Max element at pos 5
Absolute max element at pos 2
```

### See also

- **ranges::min_element (C++20)** — returns the smallest element in a range
  (niebloid)
- **ranges::minmax_element (C++20)** — returns the smallest and the largest
  elements in a range (niebloid)
- **ranges::max (C++20)** — returns the greater of the given values (niebloid)
- **max_element** — returns the largest element in a range (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/max_element*
