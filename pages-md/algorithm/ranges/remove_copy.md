# std::ranges::remove_copy, std::ranges::remove_copy_if, std::ranges::remove_copy_result, std::ranges::remove_copy_if_result

```cpp
Call signature
template< std::input_iterator I, std::sentinel_for<I> S,
          std::weakly_incrementable O, class T, class Proj = std::identity >
requires std::indirectly_copyable<I, O> &&
         std::indirect_binary_predicate<ranges::equal_to,
             std::projected<I, Proj>, const T*>
constexpr remove_copy_result<I, O>
    remove_copy( I first, S last, O result, const T& value, Proj proj = {} );  // (1) (since C++20)
template< ranges::input_range R, std::weakly_incrementable O, class T,
          class Proj = std::identity >
requires std::indirectly_copyable<ranges::iterator_t<R>, O> &&
         std::indirect_binary_predicate<ranges::equal_to,
             std::projected<ranges::iterator_t<R>, Proj>, const T*>
constexpr remove_copy_result<ranges::borrowed_iterator_t<R>, O>
    remove_copy( R&& r, O result, const T& value, Proj proj = {} );  // (2) (since C++20)
template< std::input_iterator I, std::sentinel_for<I> S,
          std::weakly_incrementable O, class Proj = std::identity,
          std::indirect_unary_predicate<std::projected<I, Proj>> Pred >
requires std::indirectly_copyable<I, O>
constexpr remove_copy_if_result<I, O>
    remove_copy_if( I first, S last, O result, Pred pred, Proj proj = {} );  // (3) (since C++20)
template< ranges::input_range R,
          std::weakly_incrementable O, class Proj = std::identity,
          std::indirect_unary_predicate<
              std::projected<ranges::iterator_t<R>, Proj>> Pred >
requires std::indirectly_copyable<ranges::iterator_t<R>, O>
constexpr remove_copy_if_result<ranges::borrowed_iterator_t<R>, O>
    remove_copy_if( R&& r, O result, Pred pred, Proj proj = {} );  // (4) (since C++20)
Helper types
template< class I, class O >
using remove_copy_result = ranges::in_out_result<I, O>;  // (5) (since C++20)
template< class I, class O >
using remove_copy_if_result = ranges::in_out_result<I, O>;  // (6) (since C++20)
```

Copies elements from the source range `[``first``,``last``)`, to the destination
range beginning at `result`, omitting the elements which (after being projected
by `proj`) satisfy specific criteria. The behavior is undefined if the source
and destination ranges overlap.

1) Ignores all elements that are equal to `value`.

3) Ignores all elements for which predicate `pred` returns `true`.

2,4) Same as (1,3), but uses `r` as the source range, as if using
   `ranges::begin(r)` as `first`, and `ranges::end(r)` as `last`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first, last** — the source range of elements
- **r** — the source range of elements
- **result** — the beginning of the destination range
- **value** — the value of the elements *not* to copy
- **comp** — the binary predicate to compare the projected elements
- **proj** — the projection to apply to the elements

### Return value

`{last, result + N}`, where `N` is the number of elements copied.

### Complexity

Exactly `ranges::distance(first, last)` applications of the corresponding
predicate `comp` and any projection `proj`.

### Notes

The algorithm is *stable*, i.e. preserves the relative order of the copied
elements.

### Possible implementation

```cpp
struct remove_copy_fn
{
    template<std::input_iterator I, std::sentinel_for<I> S, std::weakly_incrementable O,
             class T, class Proj = std::identity>
    requires std::indirectly_copyable<I, O> &&
             std::indirect_binary_predicate<ranges::equal_to,
                                            std::projected<I, Proj>, const T*>
    constexpr ranges::remove_copy_result<I, O>
        operator()(I first, S last, O result, const T& value, Proj proj = {}) const
    {
        for (; !(first == last); ++first)
            if (value != std::invoke(proj, *first))
            {
                *result = *first;
                ++result;
            }
        return {std::move(first), std::move(result)};
    }

    template<ranges::input_range R, std::weakly_incrementable O, class T,
             class Proj = std::identity>
    requires std::indirectly_copyable<ranges::iterator_t<R>, O> &&
             std::indirect_binary_predicate<ranges::equal_to,
             std::projected<ranges::iterator_t<R>, Proj>, const T*>
    constexpr ranges::remove_copy_result<ranges::borrowed_iterator_t<R>, O>
        operator()(R&& r, O result, const T& value, Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), std::move(result), value,
                       std::move(proj));
    }
};

inline constexpr remove_copy_fn remove_copy {};
```

```cpp
struct remove_copy_if_fn
{
    template<std::input_iterator I, std::sentinel_for<I> S, std::weakly_incrementable O,
             class Proj = std::identity,
             std::indirect_unary_predicate<std::projected<I, Proj>> Pred>
    requires std::indirectly_copyable<I, O>
    constexpr ranges::remove_copy_if_result<I, O>
        operator()(I first, S last, O result, Pred pred, Proj proj = {}) const
    {
        for (; first != last; ++first)
            if (false == std::invoke(pred, std::invoke(proj, *first)))
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
    constexpr ranges::remove_copy_if_result<ranges::borrowed_iterator_t<R>, O>
        operator()(R&& r, O result, Pred pred, Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), std::move(result),
                       std::move(pred), std::move(proj));
    }
};

inline constexpr remove_copy_if_fn remove_copy_if {};
```

### Example

```cpp
#include <algorithm>
#include <array>
#include <complex>
#include <iomanip>
#include <iostream>
#include <iterator>
#include <string_view>
#include <vector>

void print(const auto rem, const auto& v)
{
    std::cout << rem << ' ';
    for (const auto& e : v)
        std::cout << e << ' ';
    std::cout << '\n';
}

int main()
{
    // Filter out the hash symbol from the given string.
    const std::string_view str{"#Small #Buffer #Optimization"};
    std::cout << "before: " << std::quoted(str) << '\n';

    std::cout << "after:  \"";
    std::ranges::remove_copy(str.begin(), str.end(),
                             std::ostream_iterator<char>(std::cout), '#');
    std::cout << "\"\n";

    // Copy only the complex numbers with positive imaginary part.
    using Ci = std::complex<int>;
    constexpr std::array<Ci, 5> source
    {
        Ci{1, 0}, Ci{0, 1}, Ci{2, -1}, Ci{3, 2}, Ci{4, -3}
    };
    std::vector<std::complex<int>> target;

    std::ranges::remove_copy_if(
        source,
        std::back_inserter(target),
        [](int imag) { return imag <= 0; },
        [](Ci z) { return z.imag(); }
    );

    print("source:", source);
    print("target:", target);
}
```

Output:

```text
before: "#Small #Buffer #Optimization"
after:  "Small Buffer Optimization"
source: (1,0) (0,1) (2,-1) (3,2) (4,-3)
target: (0,1) (3,2)
```

### See also

- **ranges::removeranges::remove_if (C++20)(C++20)** — removes elements
  satisfying specific criteria (niebloid)
- **ranges::copyranges::copy_if (C++20)(C++20)** — copies a range of elements to
  a new location (niebloid)
- **ranges::copy_n (C++20)** — copies a number of elements to a new location
  (niebloid)
- **ranges::copy_backward (C++20)** — copies a range of elements in backwards
  order (niebloid)
- **ranges::replace_copyranges::replace_copy_if (C++20)(C++20)** — copies a
  range, replacing elements satisfying specific criteria with another value
  (niebloid)
- **ranges::reverse_copy (C++20)** — creates a copy of a range that is reversed
  (niebloid)
- **ranges::rotate_copy (C++20)** — copies and rotate a range of elements
  (niebloid)
- **ranges::unique_copy (C++20)** — creates a copy of some range of elements
  that contains no consecutive duplicates (niebloid)
- **remove_copyremove_copy_if** — copies a range of elements omitting those that
  satisfy specific criteria (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/remove_copy*
