# std::ranges::partial_sort

```cpp
Call signature
template< std::random_access_iterator I, std::sentinel_for<I> S,
          class Comp = ranges::less, class Proj = std::identity >
requires std::sortable<I, Comp, Proj>
constexpr I
    partial_sort( I first, I middle, S last, Comp comp = {}, Proj proj = {} );  // (1) (since C++20)
template< ranges::random_access_range R,
          class Comp = ranges::less, class Proj = std::identity >
requires std::sortable<ranges::iterator_t<R>, Comp, Proj>
constexpr ranges::borrowed_iterator_t<R>
    partial_sort( R&& r, ranges::iterator_t<R> middle, Comp comp = {},
                  Proj proj = {} );  // (2) (since C++20)
```

1) Rearranges elements such that the range `[``first``,``middle``)` contains the
   sorted `middle - first` smallest elements in the range
   `[``first``,``last``)`.

The order of equal elements is *not* guaranteed to be preserved. The order of
   the remaining elements in the range `[``middle``,``last``)` is *unspecified*.

The elements are compared using the given binary comparison function `comp` and
   projected using `proj` function object.

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

- **first, last** — iterator-sentinel defining the range to sort
- **r** — the range to sort
- **middle** — the iterator defining the last element to be sorted
- **comp** — comparator to apply to the projected elements
- **proj** — projection to apply to the elements

### Return value

An iterator equal to `last`.

### Complexity

\(\scriptsize \mathcal{O}(N\cdot\log{(M)})\)𝓞(N·log(M)) comparisons and twice as
many projections, where \(\scriptsize N\)N is `ranges::distance(first, last)`,
\(\scriptsize M\)M is `ranges::distance(first, middle)`.

### Possible implementation

```cpp
struct partial_sort_fn
{
    template<std::random_access_iterator I, std::sentinel_for<I> S,
             class Comp = ranges::less, class Proj = std::identity>
    requires std::sortable<I, Comp, Proj>
    constexpr I
        operator()(I first, I middle, S last, Comp comp = {}, Proj proj = {}) const
    {
        if (first == middle)
            return ranges::next(first, last);
        ranges::make_heap(first, middle, comp, proj);
        auto it {middle};
        for (; it != last; ++it)
        {
            if (std::invoke(comp, std::invoke(proj, *it), std::invoke(proj, *first)))
            {
                ranges::pop_heap(first, middle, comp, proj);
                ranges::iter_swap(middle - 1, it);
                ranges::push_heap(first, middle, comp, proj);
            }
        }
        ranges::sort_heap(first, middle, comp, proj);
        return it;
    }

    template<ranges::random_access_range R, class Comp = ranges::less,
             class Proj = std::identity>
    requires std::sortable<ranges::iterator_t<R>, Comp, Proj>
    constexpr ranges::borrowed_iterator_t<R>
        operator()(R&& r, ranges::iterator_t<R> middle, Comp comp = {}, Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), std::move(middle), ranges::end(r),
                       std::move(comp), std::move(proj));
    }
};

inline constexpr partial_sort_fn partial_sort {};
```

### Example

```cpp
#include <algorithm>
#include <functional>
#include <iostream>
#include <string>
#include <vector>

void print(const auto& v)
{
    for (const char e : v)
        std::cout << e << ' ';
    std::cout << '\n';
}

void underscore(int n)
{
    while (n-- > 0)
        std::cout << "^ ";
    std::cout << '\n';
}

int main()
{
    static_assert('A' < 'a');
    std::vector<char> v {'x', 'P', 'y', 'C', 'z', 'w', 'P', 'o'};
    print(v);
    const int m {3};
    std::ranges::partial_sort(v, v.begin() + m);
    print(v), underscore(m);

    static_assert('1' < 'a');
    std::string s {"3a1b41c5"};
    print(s);
    std::ranges::partial_sort(s.begin(), s.begin() + m, s.end(), std::greater {});
    print(s), underscore(m);
}
```

Output:

```text
x P y C z w P o
C P P y z x w o
^ ^ ^
3 a 1 b 4 1 c 5
c b a 1 3 1 4 5
^ ^ ^
```

### See also

- **ranges::partial_sort_copy (C++20)** — copies and partially sorts a range of
  elements (niebloid)
- **ranges::sort (C++20)** — sorts a range into ascending order (niebloid)
- **ranges::stable_sort (C++20)** — sorts a range of elements while preserving
  order between equal elements (niebloid)
- **ranges::nth_element (C++20)** — partially sorts the given range making sure
  that it is partitioned by the given element (niebloid)
- **ranges::make_heap (C++20)** — creates a max heap out of a range of elements
  (niebloid)
- **ranges::pop_heap (C++20)** — removes the largest element from a max heap
  (niebloid)
- **ranges::push_heap (C++20)** — adds an element to a max heap (niebloid)
- **ranges::sort_heap (C++20)** — turns a max heap into a range of elements
  sorted in ascending order (niebloid)
- **partial_sort** — sorts the first N elements of a range (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/partial_sort*
