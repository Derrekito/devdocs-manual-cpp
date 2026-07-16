# std::ranges::move_backward, std::ranges::move_backward_result

```cpp
Call signature
template< std::bidirectional_iterator I1, std::sentinel_for<I1> S1,
          std::bidirectional_iterator I2 >
requires std::indirectly_movable<I1, I2>
constexpr move_backward_result<I1, I2>
    move_backward( I1 first, S1 last, I2 result );  // (1) (since C++20)
template< ranges::bidirectional_range R, std::bidirectional_iterator I >
requires std::indirectly_movable<ranges::iterator_t<R>, I>
constexpr move_backward_result<ranges::borrowed_iterator_t<R>, I>
    move_backward( R&& r, I result );  // (2) (since C++20)
Helper types
template< class I, class O >
using move_backward_result = ranges::in_out_result<I, O>;  // (3) (since C++20)
```

1) Moves the elements in the range, defined by `[``first``,``last``)`, to
   another range `[``result - N``,``result``)`, where `N =
   ranges::distance(first, last)`. The elements are moved in reverse order (the
   last element is moved first), but their relative order is preserved. The
   behavior is undefined if `result` is within `(first, last]`. In such a case,
   `ranges::move` may be used instead.

2) Same as (1), but uses `r` as the source range, as if using `ranges::begin(r)`
   as `first`, and `ranges::end(r)` as `last`.

The elements in the *moved-from* range will still contain valid values of the
appropriate type, but not necessarily the same values as before the move, as if
using `*(result - n) = ranges::iter_move(last - n)` for each integer `n`, where
`0 ≤ n < N`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first** — the beginning of the range of elements to move
- **last** — the end of the range of elements to move
- **r** — the range of the elements to move
- **result** — the end of the destination range

### Return value

`{last, result - N}`.

### Complexity

1) Exactly `N` move assignments.

2) Exactly `ranges::distance(r)` move assignments.

### Notes

When moving overlapping ranges, `ranges::move` is appropriate when moving to the
left (beginning of the destination range is outside the source range) while
`ranges::move_backward` is appropriate when moving to the right (end of the
destination range is outside the source range).

### Possible implementation

```cpp
struct move_backward_fn
{
    template<std::bidirectional_iterator I1, std::sentinel_for<I1> S1,
             std::bidirectional_iterator I2>
    requires std::indirectly_movable<I1, I2>
    constexpr ranges::move_backward_result<I1, I2>
        operator()(I1 first, S1 last, I2 result) const
    {
        auto i {last};
        for (; i != first; *--result = ranges::iter_move(--i))
        {}
        return {std::move(last), std::move(result)};
    }

    template<ranges::bidirectional_range R, std::bidirectional_iterator I>
    requires std::indirectly_movable<ranges::iterator_t<R>, I>
    constexpr ranges::move_backward_result<ranges::borrowed_iterator_t<R>, I>
        operator()(R&& r, I result) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), std::move(result));
    }
};

inline constexpr move_backward_fn move_backward {};
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <string_view>
#include <vector>

using Vec = std::vector<std::string>;

void print(std::string_view rem, Vec const& vec)
{
    std::cout << rem << "[" << vec.size() << "]: ";
    for (const std::string& s : vec)
        std::cout << (s.size() ? s : std::string{"·"}) << ' ';
    std::cout << '\n';
}

int main()
{
    Vec a{"▁", "▂", "▃", "▄", "▅", "▆", "▇", "█"};
    Vec b(a.size());

    print("Before move:\n" "a", a);
    print("b", b);

    std::ranges::move_backward(a, b.end());

    print("\n" "Move a >> b:\n" "a", a);
    print("b", b);

    std::ranges::move_backward(b.begin(), b.end(), a.end());
    print("\n" "Move b >> a:\n" "a", a);
    print("b", b);

    std::ranges::move_backward(a.begin(), a.begin()+3, a.end());
    print("\n" "Overlapping move a[0, 3) >> a[5, 8):\n" "a", a);
}
```

Possible output:

```text
Before move:
a[8]: ▁ ▂ ▃ ▄ ▅ ▆ ▇ █
b[8]: · · · · · · · ·

Move a >> b:
a[8]: · · · · · · · ·
b[8]: ▁ ▂ ▃ ▄ ▅ ▆ ▇ █

Move b >> a:
a[8]: ▁ ▂ ▃ ▄ ▅ ▆ ▇ █
b[8]: · · · · · · · ·

Overlapping move a[0, 3) >> a[5, 8):
a[8]: · · · ▄ ▅ ▁ ▂ ▃
```

### See also

- **ranges::move (C++20)** — moves a range of elements to a new location
  (niebloid)
- **ranges::copyranges::copy_if (C++20)(C++20)** — copies a range of elements to
  a new location (niebloid)
- **ranges::copy_backward (C++20)** — copies a range of elements in backwards
  order (niebloid)
- **move (C++11)** — moves a range of elements to a new location (function
  template)
- **move (C++11)** — obtains an rvalue reference (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/move_backward*
