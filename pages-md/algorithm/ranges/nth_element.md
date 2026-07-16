# std::ranges::nth_element

```cpp
Call signature
template< std::random_access_iterator I, std::sentinel_for<I> S,
          class Comp = ranges::less, class Proj = std::identity >
requires std::sortable<I, Comp, Proj>
constexpr I
    nth_element( I first, I nth, S last, Comp comp = {}, Proj proj = {} );  // (1) (since C++20)
template< ranges::random_access_range R,
          class Comp = ranges::less, class Proj = std::identity >
requires std::sortable<iterator_t<R>, Comp, Proj>
constexpr ranges::borrowed_iterator_t<R>
    nth_element( R&& r, iterator_t<R> nth, Comp comp = {}, Proj proj = {} );  // (2) (since C++20)
```

Reorders the elements in `[``first``,``last``)` such that:

- The element pointed at by `nth` is changed to whatever element would occur in
  that position if `[``first``,``last``)` were sorted with respect to `comp` and
  `proj`.
- All of the elements before this new `nth` element are *less than or equal to*
  the elements after the new `nth` element. That is, for every iterator *i*, *j*
  in the ranges `[``first``,``nth``)`, `[``nth``,``last``)` respectively, the
  expression `std::invoke(comp, std::invoke(proj, *j), std::invoke(proj, *i))`
  evaluates to `false`.
- If `nth == last` then the function has no effect.

1) Elements are compared using the given binary comparison function object
   `comp` and projection object `proj`.

2) Same as (1), but uses `r` as the range, as if using `ranges::begin(r)` as
   `first` and `ranges::end(r)` as `last`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first, last** — the range of elements to reorder
- **r** — the range of elements to reorder
- **nth** — the iterator defining the partition point
- **comp** — comparator used to compare the projected elements
- **proj** — projection to apply to the elements

### Return value

1) An iterator equal to `last`.

2) Same as (1) if `r` is an lvalue or of a `borrowed_range` type. Otherwise
   returns `std::ranges::dangling`.

### Complexity

Linear in `ranges::distance(first, last)` on average.

### Notes

The algorithm used is typically introselect although other selection algorithms
with suitable average-case complexity are allowed.

### Possible implementation

See also the implementation in msvc stl, libstdc++, and libc++: (1) / (2).

### Example

```cpp
#include <algorithm>
#include <array>
#include <functional>
#include <iostream>
#include <ranges>
#include <string_view>

void print(std::string_view rem, std::ranges::input_range auto const& a)
{
    for (std::cout << rem; const auto e : a)
        std::cout << e << ' ';
    std::cout << '\n';
}

int main()
{
    std::array v{5, 6, 4, 3, 2, 6, 7, 9, 3};
    print("Before nth_element: ", v);

    std::ranges::nth_element(v, v.begin() + v.size() / 2);
    print("After nth_element:  ", v);
    std::cout << "The median is: " << v[v.size() / 2] << '\n';

    std::ranges::nth_element(v, v.begin() + 1, std::greater<int>());
    print("After nth_element:  ", v);
    std::cout << "The second largest element is: " << v[1] << '\n';
    std::cout << "The largest element is: " << v[0] << "\n\n";

    using namespace std::literals;
    std::array names
    {
        "Diva"sv, "Cornelius"sv, "Munro"sv, "Rhod"sv,
        "Zorg"sv, "Korben"sv, "Bender"sv, "Leeloo"sv,
    };
    print("Before nth_element: ", names);
    auto fifth_element{std::ranges::next(names.begin(), 4)};
    std::ranges::nth_element(names, fifth_element);
    print("After nth_element:  ", names);
    std::cout << "The 5th element is: " << *fifth_element << '\n';
}
```

Output:

```text
Before nth_element: 5 6 4 3 2 6 7 9 3
After nth_element:  2 3 3 4 5 6 6 7 9
The median is: 5
After nth_element:  9 7 6 6 5 4 3 3 2
The second largest element is: 7
The largest element is: 9

Before nth_element: Diva Cornelius Munro Rhod Zorg Korben Bender Leeloo
After nth_element:  Diva Cornelius Bender Korben Leeloo Rhod Munro Zorg
The 5th element is: Leeloo
```

### See also

- **ranges::max_element (C++20)** — returns the largest element in a range
  (niebloid)
- **ranges::min_element (C++20)** — returns the smallest element in a range
  (niebloid)
- **ranges::partition (C++20)** — divides a range of elements into two groups
  (niebloid)
- **ranges::partial_sort (C++20)** — sorts the first N elements of a range
  (niebloid)
- **nth_element** — partially sorts the given range making sure that it is
  partitioned by the given element (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/nth_element*
