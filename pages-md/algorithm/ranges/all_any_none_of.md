# std::ranges::all_of, std::ranges::any_of, std::ranges::none_of

```cpp
Call signature
template< std::input_iterator I, std::sentinel_for<I> S,
          class Proj = std::identity,
          std::indirect_unary_predicate<std::projected<I, Proj>> Pred >
constexpr bool all_of( I first, S last, Pred pred, Proj proj = {} );  // (1) (since C++20)
template< ranges::input_range R, class Proj = std::identity,
          std::indirect_unary_predicate<
              std::projected<ranges::iterator_t<R>,Proj>> Pred >
constexpr bool all_of( R&& r, Pred pred, Proj proj = {} );  // (2) (since C++20)
template< std::input_iterator I, std::sentinel_for<I> S,
          class Proj = std::identity,
          std::indirect_unary_predicate<std::projected<I, Proj>> Pred >
constexpr bool any_of( I first, S last, Pred pred, Proj proj = {} );  // (3) (since C++20)
template< ranges::input_range R, class Proj = std::identity,
          std::indirect_unary_predicate<
              std::projected<ranges::iterator_t<R>,Proj>> Pred >
constexpr bool any_of( R&& r, Pred pred, Proj proj = {} );  // (4) (since C++20)
template< std::input_iterator I, std::sentinel_for<I> S,
          class Proj = std::identity,
          std::indirect_unary_predicate<std::projected<I, Proj>> Pred >
constexpr bool none_of( I first, S last, Pred pred, Proj proj = {} );  // (5) (since C++20)
template< ranges::input_range R, class Proj = std::identity,
          std::indirect_unary_predicate<
              std::projected<ranges::iterator_t<R>,Proj>> Pred >
constexpr bool none_of( R&& r, Pred pred, Proj proj = {} );  // (6) (since C++20)
```

1) Checks if unary predicate `pred` returns `true` for all elements in the range
   `[``first``,``last``)` (after projecting with the projection `proj`).

3) Checks if unary predicate `pred` returns `true` for at least one element in
   the range `[``first``,``last``)` (after projecting with the projection
   `proj`).

5) Checks if unary predicate `pred` returns `true` for no elements in the range
   `[``first``,``last``)` (after projecting with the projection `proj`).

2,4,6) Same as (1,3,5), but uses `r` as the source range, as if using
   `ranges::begin(r)` as `first` and `ranges::end(r)` as `last`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first, last** — the range of the elements to examine
- **r** — the range of the elements to examine
- **pred** — predicate to apply to the projected elements
- **proj** — projection to apply to the elements

### Return value

1,2) `true` if `std::invoke(pred, std::invoke(proj, *i)) != false` for every
   iterator `i` in the range, `false` otherwise. Returns `true` if the range is
   empty.

3,4) `true` if `std::invoke(pred, std::invoke(proj, *i)) != false` for at least
   one iterator `i` in the range, `false` otherwise. Returns `false` if the
   range is empty.

5,6) `true` if `std::invoke(pred, std::invoke(proj, *i)) == false` for every
   iterator `i` in the range, `false` otherwise. Returns `true` if the range is
   empty.

See also Notes below.

### Complexity

At most `last - first` applications of the predicate and the projection.

### Possible implementation

```cpp
struct all_of_fn
{
    template<std::input_iterator I, std::sentinel_for<I> S, class Proj = std::identity,
             std::indirect_unary_predicate<std::projected<I, Proj>> Pred>
    constexpr bool operator()(I first, S last, Pred pred, Proj proj = {}) const
    {
        return ranges::find_if_not(first, last, std::ref(pred), std::ref(proj)) == last;
    }

    template<ranges::input_range R, class Proj = std::identity,
             std::indirect_unary_predicate<
                 std::projected<ranges::iterator_t<R>,Proj>> Pred>
    constexpr bool operator()(R&& r, Pred pred, Proj proj = {}) const
    {
        return operator()(ranges::begin(r), ranges::end(r),
                          std::ref(pred), std::ref(proj));
    }
};

inline constexpr all_of_fn all_of;
```

```cpp
struct any_of_fn
{
    template<std::input_iterator I, std::sentinel_for<I> S, class Proj = std::identity,
             std::indirect_unary_predicate<std::projected<I, Proj>> Pred>
    constexpr bool operator()(I first, S last, Pred pred, Proj proj = {}) const
    {
        return ranges::find_if(first, last, std::ref(pred), std::ref(proj)) != last;
    }

    template<ranges::input_range R, class Proj = std::identity,
             std::indirect_unary_predicate<
                 std::projected<ranges::iterator_t<R>,Proj>> Pred>
    constexpr bool operator()(R&& r, Pred pred, Proj proj = {}) const
    {
        return operator()(ranges::begin(r), ranges::end(r),
                          std::ref(pred), std::ref(proj));
    }
};

inline constexpr any_of_fn any_of;
```

```cpp
struct none_of_fn
{
    template<std::input_iterator I, std::sentinel_for<I> S, class Proj = std::identity,
             std::indirect_unary_predicate<std::projected<I, Proj>> Pred>
    constexpr bool operator()(I first, S last, Pred pred, Proj proj = {}) const
    {
        return ranges::find_if(first, last, std::ref(pred), std::ref(proj)) == last;
    }

    template<ranges::input_range R, class Proj = std::identity,
             std::indirect_unary_predicate<
                 std::projected<ranges::iterator_t<R>,Proj>> Pred>
    constexpr bool operator()(R&& r, Pred pred, Proj proj = {}) const
    {
        return operator()(ranges::begin(r), ranges::end(r),
                          std::ref(pred), std::ref(proj));
    }
};

inline constexpr none_of_fn none_of;
```

### Notes

The return value represented in the form of the Truth table is:

  input range contains
  all `true`, none `false` | some `true`, some `false` | none `true`, all
      `false` | none `true`, none `false` (empty range)
  1,2) `all_of` | `true` | `false` | `false` | `true`
  3,4) `any_of` | `true` | `true` | `false` | `false`
  5,6) `none_of` | `false` | `false` | `true` | `true`

### Example

```cpp
#include <algorithm>
#include <functional>
#include <iostream>
#include <iterator>
#include <numeric>
#include <vector>

namespace ranges = std::ranges;

constexpr bool some_of(auto&& r, auto&& pred) // some but not all
{
    return not (ranges::all_of(r, pred) or ranges::none_of(r, pred));
}

constexpr auto w = {1, 2, 3};
static_assert(!some_of(w, [](int x) { return x < 1; }));
static_assert( some_of(w, [](int x) { return x < 2; }));
static_assert(!some_of(w, [](int x) { return x < 4; }));

int main()
{
    std::vector<int> v(10, 2);
    std::partial_sum(v.cbegin(), v.cend(), v.begin());
    std::cout << "Among the numbers: ";
    ranges::copy(v, std::ostream_iterator<int>(std::cout, " "));
    std::cout << '\n';

    if (ranges::all_of(v.cbegin(), v.cend(), [](int i) { return i % 2 == 0; }))
        std::cout << "All numbers are even\n";

    if (ranges::none_of(v, std::bind(std::modulus<int>(), std::placeholders::_1, 2)))
        std::cout << "None of them are odd\n";

    auto DivisibleBy = [](int d)
    {
        return [d](int m) { return m % d == 0; };
    };

    if (ranges::any_of(v, DivisibleBy(7)))
        std::cout << "At least one number is divisible by 7\n";
}
```

Output:

```text
Among the numbers: 2 4 6 8 10 12 14 16 18 20
All numbers are even
None of them are odd
At least one number is divisible by 7
```

### See also

- **all_ofany_ofnone_of (C++11)(C++11)(C++11)** — checks if a predicate is
  `true` for all, any or none of the elements in a range (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/all_any_none_of*
