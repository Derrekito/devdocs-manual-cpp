# std::ranges::replace, std::ranges::replace_if

```cpp
Call signature
template< std::input_iterator I, std::sentinel_for<I> S, class T1, class T2,
          class Proj = std::identity >
requires std::indirectly_writable<I, const T2&> &&
         std::indirect_binary_predicate<ranges::equal_to,
             std::projected<I, Proj>, const T1*>
constexpr I
    replace( I first, S last, const T1& old_value, const T2& new_value,
             Proj proj = {} );  // (1) (since C++20)
template< ranges::input_range R, class T1, class T2, class Proj = std::identity >
requires std::indirectly_writable<ranges::iterator_t<R>, const T2&> &&
         std::indirect_binary_predicate<ranges::equal_to,
             std::projected<ranges::iterator_t<R>, Proj>, const T1*>
constexpr ranges::borrowed_iterator_t<R>
    replace( R&& r, const T1& old_value, const T2& new_value, Proj proj = {} );  // (2) (since C++20)
template< std::input_iterator I, std::sentinel_for<I> S, class T,
          class Proj = std::identity,
          std::indirect_unary_predicate<std::projected<I, Proj>> Pred >
requires std::indirectly_writable<I, const T&>
constexpr I
    replace_if( I first, S last, Pred pred, const T& new_value, Proj proj = {} );  // (3) (since C++20)
template< ranges::input_range R, class T, class Proj = std::identity,
          std::indirect_unary_predicate<
              std::projected<ranges::iterator_t<R>, Proj>> Pred >
requires std::indirectly_writable<ranges::iterator_t<R>, const T&>
constexpr ranges::borrowed_iterator_t<R>
    replace_if( R&& r, Pred pred, const T& new_value, Proj proj = {} );  // (4) (since C++20)
```

Replaces all elements satisfying specific criteria with `new_value` in the range
`[``first``,``last``)`.

1) Replaces all elements that are equal to `old_value`, using `std::invoke(proj,
   *i) == old_value` to compare.

3) Replaces all elements for which the predicate `pred` evaluates to `true`,
   where evaluating expression is `std::invoke(pred, std::invoke(proj, *i))`.

2,4) Same as (1,3), but uses `r` as the range, as if using `ranges::begin(r)` as
   `first` and `ranges::end(r)` as `last`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first, last** — the range of elements to process
- **r** — the range of elements to process
- **old_value** — the value of elements to replace
- **new_value** — the value to use as a replacement
- **pred** — predicate to apply to the projected elements
- **proj** — projection to apply to the elements

### Return value

An iterator equal to `last`.

### Complexity

Exactly `ranges::distance(first, last)` applications of the corresponding
predicate `comp` and any projection `proj`.

### Notes

Because the algorithm takes `old_value` and `new_value` by reference, it may
have unexpected behavior if either is a reference to an element of the range
`[``first``,``last``)`.

### Possible implementation

```cpp
struct replace_fn
{
    template<std::input_iterator I, std::sentinel_for<I> S, class T1, class T2,
             class Proj = std::identity>
    requires std::indirectly_writable<I, const T2&> && std::indirect_binary_predicate<
                 ranges::equal_to, std::projected<I, Proj>, const T1*>
    constexpr I
        operator()(I first, S last, const T1& old_value, const T2& new_value,
                   Proj proj = {}) const
    {
        for (; first != last; ++first)
            if (old_value == std::invoke(proj, *first))
                *first = new_value;
        return first;
    }

    template<ranges::input_range R, class T1, class T2, class Proj = std::identity>
    requires std::indirectly_writable<ranges::iterator_t<R>, const T2&> &&
             std::indirect_binary_predicate<ranges::equal_to,
             std::projected<ranges::iterator_t<R>, Proj>, const T1*>
    constexpr ranges::borrowed_iterator_t<R>
        operator()(R&& r, const T1& old_value, const T2& new_value, Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), old_value,
                       new_value, std::move(proj));
    }
};

inline constexpr replace_fn replace {};
```

```cpp
struct replace_if_fn
{
    template<std::input_iterator I, std::sentinel_for<I> S, class T,
             class Proj = std::identity, std::indirect_unary_predicate<
                 std::projected<I, Proj>> Pred>
    requires std::indirectly_writable<I, const T&>
    constexpr I
        operator()(I first, S last, Pred pred, const T& new_value, Proj proj = {}) const
    {
        for (; first != last; ++first)
            if (!!std::invoke(pred, std::invoke(proj, *first)))
                *first = new_value;
        return std::move(first);
    }

    template<ranges::input_range R, class T, class Proj = std::identity,
             std::indirect_unary_predicate<std::projected<ranges::iterator_t<R>, Proj>> Pred>
    requires std::indirectly_writable<ranges::iterator_t<R>, const T&>
    constexpr ranges::borrowed_iterator_t<R>
        operator()(R&& r, Pred pred, const T& new_value, Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), std::move(pred),
                       new_value, std::move(proj));
    }
};

inline constexpr replace_if_fn replace_if {};
```

### Example

```cpp
#include <algorithm>
#include <array>
#include <iostream>

int main()
{
    auto print = [](const auto& v)
    {
        for (const auto& e : v)
            std::cout << e << ' ';
        std::cout << '\n';
    };

    std::array p {1, 6, 1, 6, 1, 6};
    print(p);
    std::ranges::replace(p, 6, 9);
    print(p);

    std::array q {1, 2, 3, 6, 7, 8, 4, 5};
    print(q);
    std::ranges::replace_if(q, [](int x) { return 5 < x; }, 5);
    print(q);
}
```

Output:

```text
1 6 1 6 1 6
1 9 1 9 1 9
1 2 3 6 7 8 4 5
1 2 3 5 5 5 4 5
```

### See also

- **ranges::replace_copyranges::replace_copy_if (C++20)(C++20)** — copies a
  range, replacing elements satisfying specific criteria with another value
  (niebloid)
- **replacereplace_if** — replaces all values satisfying specific criteria with
  another value (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/replace*
