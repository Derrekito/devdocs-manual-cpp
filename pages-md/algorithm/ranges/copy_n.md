# std::ranges::copy_n, std::ranges::copy_n_result

```cpp
Call signature
template< std::input_iterator I, std::weakly_incrementable O >
requires std::indirectly_copyable<I, O>
constexpr copy_n_result<I, O>
    copy_n( I first, std::iter_difference_t<I> n, O result );  // (1) (since C++20)
Helper type
template< class I, class O >
using copy_n_result = ranges::in_out_result<I, O>;  // (2) (since C++20)
```

1) Copies exactly `n` values from the range beginning at `first` to the range
   beginning at `result` by performing `*(result + i) = *(first + i)` for each
   integer in `[``​0​``,``n``)`. The behavior is undefined if `result` is within
   the range `[``first``,``first + n``)` (`ranges::copy_backward` might be used
   instead in this case).

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first** — the beginning of the range of elements to copy from
- **n** — number of the elements to copy
- **result** — the beginning of the destination range

### Return value

`ranges::copy_n_result{first + n, result + n}` or more formally, a value of type
`ranges::in_out_result` that contains an `std::input_iterator` iterator equals
to `ranges::next(first, n)` and a `std::weakly_incrementable` iterator equals to
`ranges::next(result, n)`.

### Complexity

Exactly `n` assignments.

### Notes

In practice, implementations of `std::ranges::copy_n` may avoid multiple
assignments and use bulk copy functions such as `std::memmove` if the value type
is TriviallyCopyable and the iterator types satisfy `contiguous_iterator`.
Alternatively, such copy acceleration can be injected during an optimization
phase of a compiler.

When copying overlapping ranges, `std::ranges::copy_n` is appropriate when
copying to the left (beginning of the destination range is outside the source
range) while `std::ranges::copy_backward` is appropriate when copying to the
right (end of the destination range is outside the source range).

### Possible implementation

```cpp
struct copy_n_fn
{
    template<std::input_iterator I, std::weakly_incrementable O>
    requires std::indirectly_copyable<I, O>
    constexpr ranges::copy_n_result<I, O>
        operator()(I first, std::iter_difference_t<I> n, O result) const
    {
        for (std::iter_difference_t<I> i {}; i != n; ++i, ++first, ++result)
            *result = *first;
        return {std::move(first), std::move(result)};
    }
};

inline constexpr copy_n_fn copy_n {};
```

### Example

```cpp
#include <algorithm>
#include <iomanip>
#include <iostream>
#include <iterator>
#include <string>
#include <string_view>

int main()
{
    const std::string_view in {"ABCDEFGH"};
    std::string out;

    std::ranges::copy_n(in.begin(), 4, std::back_inserter(out));
    std::cout << std::quoted(out) << '\n';

    out = "abcdefgh";
    const auto res = std::ranges::copy_n(in.begin(), 5, out.begin());
    std::cout
        << "*(res.in): '" << *(res.in) << "', distance: "
        << std::distance(std::begin(in), res.in) << '\n'
        << "*(res.out): '" << *(res.out) << "', distance: "
        << std::distance(std::begin(out), res.out) << '\n';
}
```

Output:

```text
"ABCD"
*(res.in): 'F', distance: 5
*(res.out): 'f', distance: 5
```

### See also

- **ranges::copyranges::copy_if (C++20)(C++20)** — copies a range of elements to
  a new location (niebloid)
- **ranges::copy_backward (C++20)** — copies a range of elements in backwards
  order (niebloid)
- **ranges::remove_copyranges::remove_copy_if (C++20)(C++20)** — copies a range
  of elements omitting those that satisfy specific criteria (niebloid)
- **ranges::replace_copyranges::replace_copy_if (C++20)(C++20)** — copies a
  range, replacing elements satisfying specific criteria with another value
  (niebloid)
- **ranges::reverse_copy (C++20)** — creates a copy of a range that is reversed
  (niebloid)
- **ranges::rotate_copy (C++20)** — copies and rotate a range of elements
  (niebloid)
- **ranges::unique_copy (C++20)** — creates a copy of some range of elements
  that contains no consecutive duplicates (niebloid)
- **ranges::move (C++20)** — moves a range of elements to a new location
  (niebloid)
- **ranges::move_backward (C++20)** — moves a range of elements to a new
  location in backwards order (niebloid)
- **copy_n (C++11)** — copies a number of elements to a new location (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/copy_n*
