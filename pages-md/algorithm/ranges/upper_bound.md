# std::ranges::upper_bound

```cpp
Call signature
template< std::forward_iterator I, std::sentinel_for<I> S,
          class T, class Proj = std::identity,
          std::indirect_strict_weak_order<
              const T*,
              std::projected<I, Proj>> Comp = ranges::less >
constexpr I
    upper_bound( I first, S last, const T& value, Comp comp = {}, Proj proj = {} );  // (1) (since C++20)
template< ranges::forward_range R, class T, class Proj = std::identity,
          std::indirect_strict_weak_order<
              const T*,
              std::projected<ranges::iterator_t<R>, Proj>> Comp = ranges::less >
constexpr ranges::borrowed_iterator_t<R>
    upper_bound( R&& r, const T& value, Comp comp = {}, Proj proj = {} );  // (2) (since C++20)
```

1) Returns an iterator pointing to the first element in the range
   `[``first``,``last``)` that is *greater* than `value`, or `last` if no such
   element is found. The range `[``first``,``last``)` must be partitioned with
   respect to the expression or `!comp(value, element)`, i.e., all elements for
   which the expression is `true` must precede all elements for which the
   expression is `false`. A fully-sorted range meets this criterion.

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

- **first, last** — iterator-sentinel defining the partially-ordered range to
  examine
- **r** — the partially-ordered range to examine
- **value** — value to compare the elements to
- **pred** — predicate to apply to the projected elements
- **proj** — projection to apply to the elements

### Return value

Iterator pointing to the first element that is *greater* than `value`, or `last`
if no such element is found.

### Complexity

The number of comparisons and applications of the projection performed are
logarithmic in the distance between `first` and `last` (at most log2(last -
first) + O(1) comparisons and applications of the projection). However, for an
iterator that does not model `random_access_iterator`, the number of iterator
increments is linear.

### Possible implementation

```cpp
struct upper_bound_fn
{
    template<std::forward_iterator I, std::sentinel_for<I> S,
             class T, class Proj = std::identity,
             std::indirect_strict_weak_order<
                 const T*,
                 std::projected<I, Proj>> Comp = ranges::less>
    constexpr I operator()(I first, S last, const T& value,
                           Comp comp = {}, Proj proj = {}) const
    {
        I it;
        std::iter_difference_t<I> count, step;
        count = ranges::distance(first, last);

        while (count > 0)
        {
            it = first;
            step = count / 2;
            ranges::advance(it, step, last);
            if (!comp(value, std::invoke(proj, *it)))
            {
                first = ++it;
                count -= step + 1;
            }
            else
                count = step;
        }
        return first;
    }

    template<ranges::forward_range R, class T, class Proj = std::identity,
             std::indirect_strict_weak_order<
                 const T*,
                 std::projected<ranges::iterator_t<R>, Proj>> Comp = ranges::less>
    constexpr ranges::borrowed_iterator_t<R>
        operator()(R&& r, const T& value, Comp comp = {}, Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), value,
                       std::ref(comp), std::ref(proj));
    }
};

inline constexpr upper_bound_fn upper_bound;
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    namespace ranges = std::ranges;

    std::vector<int> data {1, 1, 2, 3, 3, 3, 3, 4, 4, 4, 5, 5, 6};

    {
        auto lower = ranges::lower_bound(data.begin(), data.end(), 4);
        auto upper = ranges::upper_bound(data.begin(), data.end(), 4);

        ranges::copy(lower, upper, std::ostream_iterator<int>(std::cout, " "));
        std::cout << '\n';
    }
    {
        auto lower = ranges::lower_bound(data, 3);
        auto upper = ranges::upper_bound(data, 3);

        ranges::copy(lower, upper, std::ostream_iterator<int>(std::cout, " "));
        std::cout << '\n';
    }
}
```

Output:

```text
4 4 4
3 3 3 3
```

### See also

- **ranges::equal_range (C++20)** — returns range of elements matching a
  specific key (niebloid)
- **ranges::lower_bound (C++20)** — returns an iterator to the first element
  *not less* than the given value (niebloid)
- **ranges::partition (C++20)** — divides a range of elements into two groups
  (niebloid)
- **upper_bound** — returns an iterator to the first element *greater* than a
  certain value (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/upper_bound*
