# std::ranges::distance

```cpp
Call signature
template< class I, std::sentinel_for<I> S >
    requires (!std::sized_sentinel_for<S, I>)
constexpr std::iter_difference_t<I>
    distance( I first, S last );  // (1) (since C++20)
template< class I, std::sized_sentinel_for<std::decay_t<I>> S >
constexpr std::iter_difference_t<std::decay_t<I>>
    distance( I&& first, S last );  // (2) (since C++20)
template< ranges::range R >
constexpr ranges::range_difference_t<R>
    distance( R&& r );  // (3) (since C++20)
```

1,2) Returns the number of hops from `first` to `last`.

3) Returns the size of `r` as a signed integer.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first** — iterator pointing to the first element
- **last** — sentinel denoting the end of the range `first` is an iterator to
- **r** — range to calculate the distance of

### Return value

1) The number of increments needed to go from `first` to `last`.

2) `last - static_cast<const std::decay_t<I>&>(first)`.

3) If `R` models `ranges::sized_range`, returns `ranges::size(r)`; otherwise
   `ranges::distance(ranges::begin(r), ranges::end(r))`.

### Complexity

1) Linear.

2) Constant.

3) If `R` models `ranges::sized_range` or if
   `std::sized_sentinel_for<ranges::sentinel_t<R>, ranges::iterator_t<R>>` is
   modeled, complexity is constant; otherwise linear.

### Possible implementation

```cpp
struct distance_fn
{
    template<class I, std::sentinel_for<I> S>
        requires (!std::sized_sentinel_for<S, I>)
    constexpr std::iter_difference_t<I> operator()(I first, S last) const
    {
        std::iter_difference_t<I> result = 0;
        while (first != last)
        {
            ++first;
            ++result;
        }
        return result;
    }

    template<class I, std::sized_sentinel_for<std::decay<I>> S>
    constexpr std::iter_difference_t<I> operator()(const I& first, S last) const
    {
        return last - first;
    }

    template<ranges::range R>
    constexpr ranges::range_difference_t<R> operator()(R&& r) const
    {
        if constexpr (ranges::sized_range<std::remove_cvref_t<R>>)
            return static_cast<ranges::range_difference_t<R>>(ranges::size(r));
        else
            return (*this)(ranges::begin(r), ranges::end(r));
    }
};

inline constexpr auto distance = distance_fn{};
```

### Example

```cpp
#include <cassert>
#include <forward_list>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v{3, 1, 4};
    assert(std::ranges::distance(v.begin(), v.end()) == 3);
    assert(std::ranges::distance(v.end(), v.begin()) == -3);
    assert(std::ranges::distance(v) == 3);

    std::forward_list<int> l{2, 7, 1};
    // auto size = std::ranges::size(l); // error: not a sizable range
    auto size = std::ranges::distance(l); // OK, but aware O(N) complexity
    assert(size == 3);
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3392 | C++20 | overload (1) takes iterator by value, thus move-only
      iterator lvalue with a sized sentinel was rejected | added overload (2)
  LWG 3664 | C++20 | the resolution of LWG issue 3392 made `ranges::distance`
      reject array arguments | accepts them

### See also

- **ranges::advance (C++20)** — advances an iterator by given distance or to a
  given bound (niebloid)
- **ranges::countranges::count_if (C++20)(C++20)** — returns the number of
  elements satisfying specific criteria (niebloid)
- **distance** — returns the distance between two iterators (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/ranges/distance*
