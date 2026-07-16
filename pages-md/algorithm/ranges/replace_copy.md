# std::ranges::replace_copy, std::ranges::replace_copy_if, std::ranges::replace_copy_result, std::ranges::replace_copy_if_result

```cpp
Call signature
template< std::input_iterator I, std::sentinel_for<I> S, class T1, class T2,
          std::output_iterator<const T2&> O, class Proj = std::identity >
requires std::indirectly_copyable<I, O> &&
         std::indirect_binary_predicate<ranges::equal_to,
             std::projected<I, Proj>, const T1*>
constexpr replace_copy_result<I, O>
    replace_copy( I first, S last, O result, const T1& old_value, const T2& new_value,
                  Proj proj = {} );  // (1) (since C++20)
template< ranges::input_range R, class T1, class T2,
          std::output_iterator<const T2&> O, class Proj = std::identity >
requires std::indirectly_copyable<ranges::iterator_t<R>, O> &&
         std::indirect_binary_predicate<ranges::equal_to,
             std::projected<ranges::iterator_t<R>, Proj>, const T1*>
constexpr replace_copy_result<ranges::borrowed_iterator_t<R>, O>
    replace_copy( R&& r, O result, const T1& old_value, const T2& new_value,
                  Proj proj = {} );  // (2) (since C++20)
template< std::input_iterator I, std::sentinel_for<I> S, class T,
          std::output_iterator<const T&> O, class Proj = std::identity,
          std::indirect_unary_predicate<std::projected<I, Proj>> Pred >
requires std::indirectly_copyable<I, O>
constexpr replace_copy_if_result<I, O>
    replace_copy_if( I first, S last, O result, Pred pred, const T& new_value,
                     Proj proj = {} );  // (3) (since C++20)
template< ranges::input_range R, class T, std::output_iterator<const T&> O,
          class Proj = std::identity,
          std::indirect_unary_predicate<
              std::projected<ranges::iterator_t<R>, Proj>> Pred >
requires std::indirectly_copyable<ranges::iterator_t<R>, O>
constexpr replace_copy_if_result<ranges::borrowed_iterator_t<R>, O>
    replace_copy_if( R&& r, O result, Pred pred, const T& new_value,
                     Proj proj = {} );  // (4) (since C++20)
Helper types
template< class I, class O >
using replace_copy_result = ranges::in_out_result<I, O>;  // (5) (since C++20)
template< class I, class O >
using replace_copy_if_result = ranges::in_out_result<I, O>;  // (6) (since C++20)
```

Copies the elements from the source range `[``first``,``last``)` to the
destination range beginning at `result`, replacing all elements satisfying
specific criteria with `new_value`. The behavior is undefined if the source and
destination ranges overlap.

1) Replaces all elements that are equal to `old_value`, using `std::invoke(proj,
   *(first + (i - result))) == old_value` to compare.

3) Replaces all elements for which the predicate `pred` evaluates to `true`,
   where the evaluating expression is `std::invoke(pred, std::invoke(proj,
   *(first + (i - result))))`.

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

- **first, last** — the range of elements to copy
- **r** — the range of elements to copy
- **result** — the beginning of the destination range
- **old_value** — the value of elements to replace
- **new_value** — the value to use as a replacement
- **pred** — predicate to apply to the projected elements
- **proj** — projection to apply to the elements.

### Return value

`{last, result + N}`, where

1,3) `N = ranges::distance(first, last)`;

2,4) `N = ranges::distance(r)`.

### Complexity

Exactly `N` applications of the corresponding predicate `comp` and any
projection `proj`.

### Possible implementation

```cpp
struct replace_copy_fn
{
    template<std::input_iterator I, std::sentinel_for<I> S, class T1, class T2,
             std::output_iterator<const T2&> O, class Proj = std::identity>
    requires std::indirectly_copyable<I, O> &&
             std::indirect_binary_predicate<ranges::equal_to,
                 std::projected<I, Proj>, const T1*>
    constexpr ranges::replace_copy_result<I, O>
        operator()(I first, S last, O result, const T1& old_value, const T2& new_value,
                   Proj proj = {}) const
    {
        for (; first != last; ++first, ++result)
            *result = (std::invoke(proj, *first) == old_value) ? new_value : *first;
        return {std::move(first), std::move(result)};
    }

    template<ranges::input_range R, class T1, class T2,
             std::output_iterator<const T2&> O, class Proj = std::identity>
    requires std::indirectly_copyable<ranges::iterator_t<R>, O> &&
             std::indirect_binary_predicate<ranges::equal_to,
                 std::projected<ranges::iterator_t<R>, Proj>, const T1*>
    constexpr ranges::replace_copy_result<ranges::borrowed_iterator_t<R>, O>
        operator()(R&& r, O result, const T1& old_value, const T2& new_value,
                   Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), std::move(result),
                       old_value, new_value, std::move(proj));
    }
};

inline constexpr replace_copy_fn replace_copy {};
```

```cpp
struct replace_copy_if_fn
{
    template<std::input_iterator I, std::sentinel_for<I> S, class T,
             std::output_iterator<const T&> O,
             class Proj = std::identity, std::indirect_unary_predicate<
                 std::projected<I, Proj>> Pred>
    requires std::indirectly_copyable<I, O>
    constexpr ranges::replace_copy_if_result<I, O>
        operator()(I first, S last, O result, Pred pred, const T& new_value,
                   Proj proj = {}) const
    {
        for (; first != last; ++first, ++result)
             *result = std::invoke(pred, std::invoke(proj, *first)) ? new_value : *first;
        return {std::move(first), std::move(result)};
    }

    template<ranges::input_range R, class T, std::output_iterator<const T&> O,
             class Proj = std::identity,
             std::indirect_unary_predicate<std::projected<ranges::iterator_t<R>, Proj>> Pred>
    requires std::indirectly_copyable<ranges::iterator_t<R>, O>
    constexpr ranges::replace_copy_if_result<ranges::borrowed_iterator_t<R>, O>
        operator()(R&& r, O result, Pred pred, const T& new_value,
                   Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), std::move(result),
                       std::move(pred), new_value, std::move(proj));
    }
};

inline constexpr replace_copy_if_fn replace_copy_if {};
```

### Example

```cpp
#include <algorithm>
#include <array>
#include <iostream>
#include <vector>

int main()
{
    auto print = [](const auto rem, const auto& v)
    {
        for (std::cout << rem << ": "; const auto& e : v)
            std::cout << e << ' ';
        std::cout << '\n';
    };

    std::vector<int> o;

    std::array p {1, 6, 1, 6, 1, 6};
    o.resize(p.size());
    print("p", p);
    std::ranges::replace_copy(p, o.begin(), 6, 9);
    print("o", o);

    std::array q {1, 2, 3, 6, 7, 8, 4, 5};
    o.resize(q.size());
    print("q", q);
    std::ranges::replace_copy_if(q, o.begin(), [](int x) { return 5 < x; }, 5);
    print("o", o);
}
```

Output:

```text
p: 1 6 1 6 1 6
o: 1 9 1 9 1 9
q: 1 2 3 6 7 8 4 5
o: 1 2 3 5 5 5 4 5
```

### See also

- **ranges::replaceranges::replace_if (C++20)(C++20)** — replaces all values
  satisfying specific criteria with another value (niebloid)
- **replace_copyreplace_copy_if** — copies a range, replacing elements
  satisfying specific criteria with another value (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/replace_copy*
