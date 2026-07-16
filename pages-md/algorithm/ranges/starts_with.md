# std::ranges::starts_with

```cpp
Call signature
template< std::input_iterator I1, std::sentinel_for<I1> S1,
          std::input_iterator I2, std::sentinel_for<I2> S2,
          class Pred = ranges::equal_to,
          class Proj1 = std::identity, class Proj2 = std::identity >
requires std::indirectly_comparable<I1, I2, Pred, Proj1, Proj2>
constexpr bool starts_with( I1 first1, S1 last1, I2 first2, S2 last2,
                            Pred pred = {}, Proj1 proj1 = {}, Proj2 proj2 = {} );  // (1) (since C++23)
template< ranges::input_range R1, ranges::input_range R2,
          class Pred = ranges::equal_to,
          class Proj1 = std::identity, class Proj2 = std::identity >
requires std::indirectly_comparable<ranges::iterator_t<R1>,
                                    ranges::iterator_t<R2>,
                                    Pred, Proj1, Proj2>
constexpr bool starts_with( R1&& r1, R2&& r2,
                            Pred pred = {}, Proj1 proj1 = {}, Proj2 proj2 = {} );  // (2) (since C++23)
```

Checks whether the second range matches the prefix of the first range.

1) Let `N1` and `N2` denote the size of ranges `[``first1``,``last1``)` and
   `[``first2``,``last2``)` respectively. If `N1 < N2`, returns `false`.
   Otherwise, returns `true` only if every element in the range
   `[``first2``,``last2``)` is equal to the corresponding element in
   `[``first1``,``first1 + N2``)`. Comparison is done by applying the binary
   predicate `pred` to elements in two ranges projected by `proj1` and `proj2`
   respectively.

2) Same as (1), but uses `r1` and `r2` as the source ranges, as if using
   `ranges::begin(r1)` as `first1`, `ranges:begin(r2)` as `first2`,
   `ranges::end(r1)` as `last1`, and `ranges::end(r2)` as `last2`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first1, last1** — the range of elements to examine
- **r1** — the range of elements to examine
- **first2, last2** — the range of elements to be used as the prefix
- **r2** — the range of elements to be used as the prefix
- **pred** — the binary predicate that compares the projected elements
- **proj1** — the projection to apply to the elements of the range to examine
- **proj2** — the projection to apply to the elements of the range to be used as
  the prefix

### Return value

`true` if the second range matches the prefix of the first range, `false`
otherwise.

### Complexity

Linear: at most `min(N1, N2)` applications of the predicate and both
projections.

### Possible implementation

```cpp
struct starts_with_fn
{
    template<std::input_iterator I1, std::sentinel_for<I1> S1,
             std::input_iterator I2, std::sentinel_for<I2> S2,
             class Pred = ranges::equal_to,
             class Proj1 = std::identity, class Proj2 = std::identity>
    requires std::indirectly_comparable<I1, I2, Pred, Proj1, Proj2>
    constexpr bool operator()(I1 first1, S1 last1, I2 first2, S2 last2,
                              Pred pred = {}, Proj1 proj1 = {}, Proj2 proj2 = {}) const
    {
        return ranges::mismatch(std::move(first1), last1, std::move(first2), last2,
                                std::move(pred), std::move(proj1), std::move(proj2)
                               ).in2 == last2;
    }

    template<ranges::input_range R1, ranges::input_range R2,
             class Pred = ranges::equal_to,
             class Proj1 = std::identity, class Proj2 = std::identity>
    requires std::indirectly_comparable<ranges::iterator_t<R1>,
                                        ranges::iterator_t<R2>,
                                        Pred, Proj1, Proj2>
    constexpr bool operator()(R1&& r1, R2&& r2,
                              Pred pred = {}, Proj1 proj1 = {}, Proj2 proj2 = {}) const
    {
        return (*this)(ranges::begin(r1), ranges::end(r1),
                       ranges::begin(r2), ranges::end(r2),
                       std::move(pred), std::move(proj1), std::move(proj2));
    }
};

inline constexpr starts_with_fn starts_with {};
```

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges_starts_ends_with` | 202106L | (C++23) |
      `std::ranges::starts_with`, `std::ranges::ends_with`

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <ranges>
#include <string_view>

int main()
{
    using namespace std::literals;

    constexpr auto ascii_upper = [](char8_t c)
    {
        return u8'a' <= c && c <= u8'z' ? static_cast<char8_t>(c + u8'A' - u8'a') : c;
    };

    constexpr auto cmp_ignore_case = [=](char8_t x, char8_t y)
    {
        return ascii_upper(x) == ascii_upper(y);
    };

    static_assert(std::ranges::starts_with("const_cast", "const"sv));
    static_assert(std::ranges::starts_with("constexpr", "const"sv));
    static_assert(!std::ranges::starts_with("volatile", "const"sv));

    std::cout << std::boolalpha
              << std::ranges::starts_with(u8"Constantinopolis", u8"constant"sv,
                                          {}, ascii_upper, ascii_upper) << ' '
              << std::ranges::starts_with(u8"Istanbul", u8"constant"sv,
                                          {}, ascii_upper, ascii_upper) << ' '
              << std::ranges::starts_with(u8"Metropolis", u8"metro"sv,
                                          cmp_ignore_case) << ' '
              << std::ranges::starts_with(u8"Acropolis", u8"metro"sv,
                                          cmp_ignore_case) << '\n';

    constexpr static auto v = { 1, 3, 5, 7, 9 };
    constexpr auto odd = [](int x) { return x % 2; };
    static_assert(std::ranges::starts_with(v, std::views::iota(1)
                                            | std::views::filter(odd)
                                            | std::views::take(3)));
}
```

Output:

```text
true false true false
```

### See also

- **ranges::ends_with (C++23)** — checks whether a range ends with another range
  (niebloid)
- **ranges::mismatch (C++20)** — finds the first position where two ranges
  differ (niebloid)
- **starts_with (C++20)** — checks if the string starts with the given prefix
  (public member function of `std::basic_string<CharT,Traits,Allocator>`)
- **starts_with (C++20)** — checks if the string view starts with the given
  prefix (public member function of `std::basic_string_view<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/starts_with*
