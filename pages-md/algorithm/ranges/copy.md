# std::ranges::copy, std::ranges::copy_if, std::ranges::copy_result, std::ranges::copy_if_result

```cpp
Call signature
template< std::input_iterator I, std::sentinel_for<I> S, std::weakly_incrementable O >
requires std::indirectly_copyable<I, O>
constexpr copy_result<I, O>
    copy( I first, S last, O result );  // (1) (since C++20)
template< ranges::input_range R, std::weakly_incrementable O >
requires std::indirectly_copyable<ranges::iterator_t<R>, O>
constexpr copy_result<ranges::borrowed_iterator_t<R>, O>
    copy( R&& r, O result );  // (2) (since C++20)
template< std::input_iterator I, std::sentinel_for<I> S, std::weakly_incrementable O,
          class Proj = std::identity,
          std::indirect_unary_predicate<std::projected<I, Proj>> Pred >
requires std::indirectly_copyable<I, O>
constexpr copy_if_result<I, O>
    copy_if( I first, S last, O result, Pred pred, Proj proj = {} );  // (3) (since C++20)
template< ranges::input_range R, std::weakly_incrementable O,
          class Proj = std::identity,
          std::indirect_unary_predicate<
              std::projected<ranges::iterator_t<R>, Proj>> Pred >
requires std::indirectly_copyable<ranges::iterator_t<R>, O>
constexpr copy_if_result<ranges::borrowed_iterator_t<R>, O>
    copy_if( R&& r, O result, Pred pred, Proj proj = {} );  // (4) (since C++20)
Helper types
template< class I, class O >
using copy_result = ranges::in_out_result<I, O>;  // (5) (since C++20)
template< class I, class O >
using copy_if_result = ranges::in_out_result<I, O>;  // (6) (since C++20)
```

Copies the elements in the range, defined by `[``first``,``last``)`, to another
range beginning at `result`.

1) Copies all elements in the range `[``first``,``last``)` starting from `first`
   and proceeding to `last - 1`. The behavior is undefined if `result` is within
   the range `[``first``,``last``)`. In this case, `ranges::copy_backward` may
   be used instead.

3) Only copies the elements for which the predicate `pred` returns `true`. The
   relative order of the elements that are copied is preserved. The behavior is
   undefined if the source and the destination ranges overlap.

2,4) Same as (1,3), but uses `r` as the source range, as if using
   `ranges::begin(r)` as `first` and `ranges::end(r)` as `last`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first, last** — the range of elements to copy
- **r** — the range of elements to copy
- **result** — the beginning of the destination range.
- **pred** — predicate to apply to the projected elements
- **proj** — projection to apply to the elements

### Return value

A `ranges::in_out_result` containing an input iterator equal to `last` and an
output iterator past the last element copied.

### Complexity

1,2) Exactly `last - first` assignments.

3,4) Exactly `last - first` applications of the predicate and projection,
   between `​0​` and `last - first` assignments (assignment for every element
   for which predicate returns `true`, dependent on predicate and input data).

### Notes

In practice, implementations of `ranges::copy` avoid multiple assignments and
use bulk copy functions such as `std::memmove` if the value type is
TriviallyCopyable and the iterator types satisfy `contiguous_iterator`.

When copying overlapping ranges, `ranges::copy` is appropriate when copying to
the left (beginning of the destination range is outside the source range) while
`ranges::copy_backward` is appropriate when copying to the right (end of the
destination range is outside the source range).

### Possible implementation

```cpp
struct copy_fn
{
    template<std::input_iterator I, std::sentinel_for<I> S, std::weakly_incrementable O>
    requires std::indirectly_copyable<I, O>
    constexpr ranges::copy_result<I, O> operator()(I first, S last, O result) const
    {
        for (; first != last; ++first, (void)++result)
            *result = *first;
        return {std::move(first), std::move(result)};
    }

    template<ranges::input_range R, std::weakly_incrementable O>
    requires std::indirectly_copyable<ranges::iterator_t<R>, O>
    constexpr ranges::copy_result<ranges::borrowed_iterator_t<R>, O>
        operator()(R&& r, O result) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), std::move(result));
    }
};

inline constexpr copy_fn copy;
```

```cpp
struct copy_if_fn
{
    template<std::input_iterator I, std::sentinel_for<I> S, std::weakly_incrementable O,
             class Proj = std::identity,
             std::indirect_unary_predicate<std::projected<I, Proj>> Pred>
    requires std::indirectly_copyable<I, O>
    constexpr ranges::copy_if_result<I, O>
        operator()(I first, S last, O result, Pred pred, Proj proj = {}) const
    {
        for (; first != last; ++first)
            if (std::invoke(pred, std::invoke(proj, *first)))
            {
                *result = *first;
                ++result;
            }
        return {std::move(first), std::move(result)};
    }

    template<ranges::input_range R, std::weakly_incrementable O,
             class Proj = std::identity,
             std::indirect_unary_predicate<
                 std::projected<ranges::iterator_t<R>, Proj>> Pred>
    requires std::indirectly_copyable<ranges::iterator_t<R>, O>
    constexpr ranges::copy_if_result<ranges::borrowed_iterator_t<R>, O>
        operator()(R&& r, O result, Pred pred, Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r),
                       std::move(result),
                       std::ref(pred), std::ref(proj));
    }
};

inline constexpr copy_if_fn copy_if;
```

### Example

The following code uses `ranges::copy` to both copy the contents of one
`std::vector` to another and to display the resulting `std::vector`:

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <numeric>
#include <vector>

int main()
{
    std::vector<int> source(10);
    std::iota(source.begin(), source.end(), 0);

    std::vector<int> destination;

    std::ranges::copy(source.begin(), source.end(),
                      std::back_inserter(destination));
// or, alternatively,
//  std::vector<int> destination(source.size());
//  std::ranges::copy(source.begin(), source.end(), destination.begin());
// either way is equivalent to
//  std::vector<int> destination = source;

    std::cout << "destination contains: ";

    std::ranges::copy(destination, std::ostream_iterator<int>(std::cout, " "));
    std::cout << '\n';

    std::cout << "odd numbers in destination are: ";

    std::ranges::copy_if(destination, std::ostream_iterator<int>(std::cout, " "),
                         [](int x) { return (x % 2) == 1; });
    std::cout << '\n';
}
```

Output:

```text
destination contains: 0 1 2 3 4 5 6 7 8 9
odd numbers in destination are: 1 3 5 7 9
```

### See also

- **ranges::copy_backward (C++20)** — copies a range of elements in backwards
  order (niebloid)
- **ranges::reverse_copy (C++20)** — creates a copy of a range that is reversed
  (niebloid)
- **ranges::copy_n (C++20)** — copies a number of elements to a new location
  (niebloid)
- **ranges::fill (C++20)** — assigns a range of elements a certain value
  (niebloid)
- **ranges::remove_copyranges::remove_copy_if (C++20)(C++20)** — copies a range
  of elements omitting those that satisfy specific criteria (niebloid)
- **copycopy_if (C++11)** — copies a range of elements to a new location
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/copy*
