# std::ranges::partition_copy, std::ranges::partition_copy_result

```cpp
Call signature
template< std::input_iterator I, std::sentinel_for<I> S,
          std::weakly_incrementable O1, std::weakly_incrementable O2,
          class Proj = std::identity,
          std::indirect_unary_predicate<std::projected<I, Proj>> Pred >
requires std::indirectly_copyable<I, O1> &&
         std::indirectly_copyable<I, O2>
constexpr partition_copy_result<I, O1, O2>
    partition_copy( I first, S last, O1 out_true, O2 out_false,
                    Pred pred, Proj proj = {} );  // (1) (since C++20)
template< ranges::input_range R,
          std::weakly_incrementable O1, std::weakly_incrementable O2,
          class Proj = std::identity,
          std::indirect_unary_predicate<std::projected<iterator_t<R>, Proj>> Pred >
requires std::indirectly_copyable<ranges::iterator_t<R>, O1> &&
         std::indirectly_copyable<ranges::iterator_t<R>, O2>
constexpr partition_copy_result<ranges::borrowed_iterator_t<R>, O1, O2>
    partition_copy( R&& r, O1 out_true, O2 out_false,
                    Pred pred, Proj proj = {} );  // (2) (since C++20)
Helper types
template< class I, class O1, class O2 >
using partition_copy_result = ranges::in_out_out_result<I, O1, O2>;  // (3) (since C++20)
```

1) Copies the elements from the input range `[``first``,``last``)` to two
   different output ranges depending on the value returned by the predicate
   `pred`. The elements that satisfy the predicate `pred` after projection by
   `proj` are copied to the range beginning at `out_true`. The rest of the
   elements are copied to the range beginning at `out_false`. The behavior is
   undefined if the input range overlaps either of the output ranges.

2) Same as (1), but uses `r` as the source range, as if using `ranges::begin(r)`
   as `first`, and `ranges::end(r)` as `last`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first, last** — the input range of elements to copy from
- **r** — the input range of elements to copy from
- **out_true** — the beginning of the output range for the elements that satisfy
  `pred`
- **out_false** — the beginning of the output range for the elements that do not
  satisfy `pred`
- **pred** — predicate to apply to the projected elements
- **proj** — projection to apply to the elements

### Return value

`{last, o1, o2}`, where `o1` and `o2` are the ends of the output ranges
respectively, after the copying is complete.

### Complexity

Exactly `ranges::distance(first, last)` applications of the corresponding
predicate `comp` and any projection `proj`.

### Possible implementation

```cpp
struct partition_copy_fn
{
    template<std::input_iterator I, std::sentinel_for<I> S,
             std::weakly_incrementable O1, std::weakly_incrementable O2,
             class Proj = std::identity, std::indirect_unary_predicate<
             std::projected<I, Proj>> Pred>
    requires std::indirectly_copyable<I, O1> && std::indirectly_copyable<I, O2>
    constexpr ranges::partition_copy_result<I, O1, O2>
        operator()(I first, S last, O1 out_true, O2 out_false,
                   Pred pred, Proj proj = {}) const
    {
        for (; first != last; ++first)
            if (!!std::invoke(pred, std::invoke(proj, *first)))
                *out_true = *first, ++out_true;
            else
                *out_false = *first, ++out_false;
        return {std::move(first), std::move(out_true), std::move(out_false)};
    }

    template<ranges::input_range R,
             std::weakly_incrementable O1, std::weakly_incrementable O2,
             class Proj = std::identity,
             std::indirect_unary_predicate<std::projected<iterator_t<R>, Proj>> Pred>
    requires std::indirectly_copyable<ranges::iterator_t<R>, O1> &&
             std::indirectly_copyable<ranges::iterator_t<R>, O2>
    constexpr ranges::partition_copy_result<ranges::borrowed_iterator_t<R>, O1, O2>
        operator()(R&& r, O1 out_true, O2 out_false, Pred pred, Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), std::move(out_true),
                       std::move(out_false), std::move(pred), std::move(proj));
    }
};

inline constexpr partition_copy_fn partition_copy {};
```

### Example

```cpp
#include <algorithm>
#include <cctype>
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    const auto in = {'N', '3', 'U', 'M', '1', 'B', '4', 'E', '1', '5', 'R', '9'};

    std::vector<int> o1(size(in)), o2(size(in));

    auto pred = [](char c) { return std::isalpha(c); };

    auto ret = std::ranges::partition_copy(in, o1.begin(), o2.begin(), pred);

    std::ostream_iterator<char> cout {std::cout, " "};
    std::cout << "in = ";
    std::ranges::copy(in, cout);
    std::cout << "\no1 = ";
    std::copy(o1.begin(), ret.out1, cout);
    std::cout << "\no2 = ";
    std::copy(o2.begin(), ret.out2, cout);
    std::cout << '\n';
}
```

Output:

```text
in = N 3 U M 1 B 4 E 1 5 R 9
o1 = N U M B E R
o2 = 3 1 4 1 5 9
```

### See also

- **ranges::partition (C++20)** — divides a range of elements into two groups
  (niebloid)
- **ranges::stable_partition (C++20)** — divides elements into two groups while
  preserving their relative order (niebloid)
- **ranges::copyranges::copy_if (C++20)(C++20)** — copies a range of elements to
  a new location (niebloid)
- **ranges::remove_copyranges::remove_copy_if (C++20)(C++20)** — copies a range
  of elements omitting those that satisfy specific criteria (niebloid)
- **partition_copy (C++11)** — copies a range dividing the elements into two
  groups (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/partition_copy*
