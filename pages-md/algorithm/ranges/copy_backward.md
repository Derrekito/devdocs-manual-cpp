# std::ranges::copy_backward, std::ranges::copy_backward_result

```cpp
Call signature
template< std::bidirectional_iterator I1, std::sentinel_for<I1> S1,
          std::bidirectional_iterator I2 >
requires std::indirectly_copyable<I1, I2>
constexpr copy_backward_result<I1, I2>
    copy_backward( I1 first, S1 last, I2 result );  // (1) (since C++20)
template< ranges::bidirectional_range R, std::bidirectional_iterator I >
requires std::indirectly_copyable<ranges::iterator_t<R>, I>
constexpr copy_backward_result<ranges::borrowed_iterator_t<R>, I>
    copy_backward( R&& r, I result );  // (2) (since C++20)
Helper types
template< class I1, class I2 >
using copy_backward_result = ranges::in_out_result<I1, I2>;  // (3) (since C++20)
```

1) Copies the elements from the range, defined by `[``first``,``last``)`, to
   another range `[``result - N``,``result``)`, where `N =
   ranges::distance(first, last)`. The elements are copied in reverse order (the
   last element is copied first), but their relative order is preserved. The
   behavior is undefined if `result` is within `(first, last]`. In such a case
   `std::ranges::copy` can be used instead.

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

- **first** — the beginning of the range of elements to copy from
- **last** — the end of the range of elements to copy from
- **r** — the range of the elements to copy from
- **result** — the end of the destination range

### Return value

`{last, result - N}`

### Complexity

Exactly `N` assignments.

### Notes

When copying overlapping ranges, `ranges::copy` is appropriate when copying to
the left (beginning of the destination range is outside the source range) while
`ranges::copy_backward` is appropriate when copying to the right (end of the
destination range is outside the source range).

### Possible implementation

```cpp
struct copy_backward_fn
{
    template<std::bidirectional_iterator I1, std::sentinel_for<I1> S1,
             std::bidirectional_iterator I2>
    requires std::indirectly_copyable<I1, I2>
    constexpr ranges::copy_backward_result<I1, I2>
        operator()(I1 first, S1 last, I2 result) const
    {
        I1 last1 {ranges::next(first, std::move(last))};
        for (I1 i {last1}; i != first;)
            *--result = *--i;
        return {std::move(last1), std::move(result)};
    }

    template<ranges::bidirectional_range R, std::bidirectional_iterator I>
    requires std::indirectly_copyable<ranges::iterator_t<R>, I>
    constexpr ranges::copy_backward_result<ranges::borrowed_iterator_t<R>, I>
        operator()(R&& r, I result) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), std::move(result));
    }
};

inline constexpr copy_backward_fn copy_backward{};
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <ranges>
#include <string_view>
#include <vector>

void print(std::string_view rem, std::ranges::forward_range auto const& r)
{
    for (std::cout << rem << ": "; auto const& elem : r)
        std::cout << elem << ' ';
    std::cout << '\n';
}

int main()
{
    const auto src = {1, 2, 3, 4};
    print("src", src);

    std::vector<int> dst(src.size() + 2);
    std::ranges::copy_backward(src, dst.end());
    print("dst", dst);

    std::ranges::fill(dst, 0);
    const auto [in, out] =
        std::ranges::copy_backward(src.begin(), src.end() - 2, dst.end());
    print("dst", dst);

    std::cout
        << "(in - src.begin) == " << std::distance(src.begin(), in) << '\n'
        << "(out - dst.begin) == " << std::distance(dst.begin(), out) << '\n';
}
```

Output:

```text
src: 1 2 3 4
dst: 0 0 1 2 3 4
dst: 0 0 0 0 1 2
(in - src.begin) == 2
(out - dst.begin) == 4
```

### See also

- **ranges::copyranges::copy_if (C++20)(C++20)** — copies a range of elements to
  a new location (niebloid)
- **ranges::copy_n (C++20)** — copies a number of elements to a new location
  (niebloid)
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
- **copy_backward** — copies a range of elements in backwards order (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/copy_backward*
