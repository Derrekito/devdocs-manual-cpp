# std::ranges::find_last, std::ranges::find_last_if, std::ranges::find_last_if_not

```cpp
Call signature
template< std::forward_iterator I, std::sentinel_for<I> S,
          class T, class Proj = std::identity >
requires std::indirect_binary_predicate<ranges::equal_to, std::projected<I, Proj>,
                                        const T*>
constexpr ranges::subrange<I>
    find_last( I first, S last, const T& value, Proj proj = {} );  // (1) (since C++23)
template< ranges::forward_range R, class T, class Proj = std::identity >
requires std::indirect_binary_predicate<ranges::equal_to,
                                        std::projected<ranges::iterator_t<R>, Proj>,
                                        const T*>
constexpr ranges::borrowed_subrange_t<R>
    find_last( R&& r, const T& value, Proj proj = {} );  // (2) (since C++23)
template< std::forward_iterator I, std::sentinel_for<I> S,
          class Proj = std::identity,
          std::indirect_unary_predicate<std::projected<I, Proj>> Pred >
constexpr ranges::subrange<I>
    find_last_if( I first, S last, Pred pred, Proj proj = {} );  // (3) (since C++23)
template< ranges::forward_range R, class Proj = std::identity,
          std::indirect_unary_predicate<std::projected<ranges::iterator_t<R>, Proj>>
              Pred >
constexpr ranges::borrowed_subrange_t<R>
    find_last_if( R&& r, Pred pred, Proj proj = {} );  // (4) (since C++23)
template< std::forward_iterator I, std::sentinel_for<I> S,
          class Proj = std::identity,
          std::indirect_unary_predicate<std::projected<I, Proj>> Pred >
constexpr ranges::subrange<I>
    find_last_if_not( I first, S last, Pred pred, Proj proj = {} );  // (5) (since C++23)
template< ranges::forward_range R, class Proj = std::identity,
          std::indirect_unary_predicate<std::projected<ranges::iterator_t<R>, Proj>>
              Pred >
constexpr ranges::borrowed_subrange_t<R>
    find_last_if_not( R&& r, Pred pred, Proj proj = {} );  // (6) (since C++23)
```

Returns the last element in the range `[``first``,``last``)` that satisfies
specific criteria:

1) `find_last` searches for an element equal to `value`.

3) `find_last_if` searches for the last element in the range
   `[``first``,``last``)` for which predicate `pred` returns `true`.

5) `find_last_if_not` searches for the last element in the range
   `[``first``,``last``)` for which predicate `pred` returns `false`.

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

- **first, last** — the range of elements to examine
- **r** — the range of the elements to examine
- **value** — value to compare the elements to
- **pred** — predicate to apply to the projected elements
- **proj** — projection to apply to the elements

### Return value

1,2,3) Let `i` be the last iterator in the range `[``first``,``last``)` for
   which `E` is `true`. Returns `ranges::subrange<I>{i, last}`, or
   `ranges::subrange<I>{last, last}` if no such iterator is found.

2,4,6) Same as (1,2,3) but the return type is `ranges::borrowed_subrange_t<I>`.

### Complexity

At most `last - first` applications of the predicate and projection.

### Notes

`ranges::find_last`, `ranges::find_last_if`, `ranges::find_last_if_not` have
better efficiency on common implementations if `I` models
`bidirectional_iterator` or (better) `random_access_iterator`.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges_find_last` | 202207L | (C++23) | `ranges::find_last`,
      `ranges::find_last_if`, `ranges::find_last_if_not`

### Possible implementation

These implementations only show the slower algorithm used when `I` models
`forward_iterator`.

```cpp
struct find_last_fn
{
    template<std::forward_iterator I, std::sentinel_for<I> S,
             class T, class Proj = std::identity>
    requires std::indirect_binary_predicate<ranges::equal_to, std::projected<I, Proj>,
                                            const T*>
    constexpr ranges::subrange<I>
        operator()(I first, S last, const T &value, Proj proj = {}) const
    {
        // Note: if I is mere forward_iterator, we may only go from begin to end.
        I found {};
        for (; first != last; ++first)
            if (std::invoke(proj, *first) == value)
                found = first;

        if (found == I {})
            return {first, first};

        return {found, std::ranges::next(found, last)};
    }

    template<ranges::forward_range R, class T, class Proj = std::identity>
    requires std::indirect_binary_predicate<ranges::equal_to,
                                            std::projected<ranges::iterator_t<R>, Proj>,
                                            const T*>
    constexpr ranges::borrowed_subrange_t<R>
        operator()(R&& r, const T &value, Proj proj = {}) const
    {
        return this->operator()(ranges::begin(r), ranges::end(r), value, std::ref(proj));
    }
};

inline constexpr find_last_fn find_last;
```

```cpp
struct find_last_if_fn
{
    template<std::forward_iterator I, std::sentinel_for<I> S,
             class Proj = std::identity,
             std::indirect_unary_predicate<std::projected<I, Proj>> Pred>
    constexpr ranges::subrange<I>
        operator()(I first, S last, Pred pred, Proj proj = {}) const
    {
        // Note: if I is mere forward_iterator, we may only go from begin to end.
        I found {};
        for (; first != last; ++first)
            if (std::invoke(pred, std::invoke(proj, *first)))
                found = first;

        if (found == I {})
            return {first, first};

        return {found, std::ranges::next(found, last)};
    }

    template<ranges::forward_range R, class Proj = std::identity,
             std::indirect_unary_predicate<std::projected<ranges::iterator_t<R>, Proj>>
                 Pred>
    constexpr ranges::borrowed_subrange_t<R>
        operator()(R&& r, Pred pred, Proj proj = {}) const
    {
        return this->operator()(ranges::begin(r), ranges::end(r),
                                std::ref(pred), std::ref(proj));
    }
};

inline constexpr find_last_if_fn find_last_if;
```

```cpp
struct find_last_if_not_fn
{
    template<std::forward_iterator I, std::sentinel_for<I> S,
             class Proj = std::identity,
             std::indirect_unary_predicate<std::projected<I, Proj>> Pred>
    constexpr ranges::subrange<I>
        operator()(I first, S last, Pred pred, Proj proj = {}) const
    {
        // Note: if I is mere forward_iterator, we may only go from begin to end.
        I found {};
        for (; first != last; ++first)
            if (!std::invoke(pred, std::invoke(proj, *first)))
                found = first;

        if (found == I {})
            return {first, first};

        return {found, std::ranges::next(found, last)};
    }

    template<ranges::forward_range R, class Proj = std::identity,
             std::indirect_unary_predicate<std::projected<ranges::iterator_t<R>, Proj>>
                 Pred>
    constexpr ranges::borrowed_subrange_t<R>
        operator()(R&& r, Pred pred, Proj proj = {}) const
    {
        return this->operator()(ranges::begin(r), ranges::end(r),
                                std::ref(pred), std::ref(proj));
    }
};

inline constexpr find_last_if_not_fn find_last_if_not;
```

### Example

```cpp
#include <algorithm>
#include <forward_list>
#include <iomanip>
#include <iostream>
#include <string_view>

int main()
{
    constexpr static auto v = {1, 2, 3, 1, 2, 3, 1, 2};

    {
        constexpr auto i1 = std::ranges::find_last(v.begin(), v.end(), 3);
        constexpr auto i2 = std::ranges::find_last(v, 3);
        static_assert(std::ranges::distance(v.begin(), i1.begin()) == 5);
        static_assert(std::ranges::distance(v.begin(), i2.begin()) == 5);
    }
    {
        constexpr auto i1 = std::ranges::find_last(v.begin(), v.end(), -3);
        constexpr auto i2 = std::ranges::find_last(v, -3);
        static_assert(i1.begin() == v.end());
        static_assert(i2.begin() == v.end());
    }

    auto abs = [](int x) { return x < 0 ? -x : x; };

    {
        auto pred = [](int x) { return x == 3; };
        constexpr auto i1 = std::ranges::find_last_if(v.begin(), v.end(), pred, abs);
        constexpr auto i2 = std::ranges::find_last_if(v, pred, abs);
        static_assert(std::ranges::distance(v.begin(), i1.begin()) == 5);
        static_assert(std::ranges::distance(v.begin(), i2.begin()) == 5);
    }
    {
        auto pred = [](int x) { return x == -3; };
        constexpr auto i1 = std::ranges::find_last_if(v.begin(), v.end(), pred, abs);
        constexpr auto i2 = std::ranges::find_last_if(v, pred, abs);
        static_assert(i1.begin() == v.end());
        static_assert(i2.begin() == v.end());
    }

    {
        auto pred = [](int x) { return x == 1 or x == 2; };
        constexpr auto i1 = std::ranges::find_last_if_not(v.begin(), v.end(), pred, abs);
        constexpr auto i2 = std::ranges::find_last_if_not(v, pred, abs);
        static_assert(std::ranges::distance(v.begin(), i1.begin()) == 5);
        static_assert(std::ranges::distance(v.begin(), i2.begin()) == 5);
    }
    {
        auto pred = [](int x) { return x == 1 or x == 2 or x == 3; };
        constexpr auto i1 = std::ranges::find_last_if_not(v.begin(), v.end(), pred, abs);
        constexpr auto i2 = std::ranges::find_last_if_not(v, pred, abs);
        static_assert(i1.begin() == v.end());
        static_assert(i2.begin() == v.end());
    }

    using P = std::pair<std::string_view, int>;
    std::forward_list<P> list
    {
        {"one", 1}, {"two", 2}, {"three", 3},
        {"one", 4}, {"two", 5}, {"three", 6},
    };
    auto cmp_one = [](const std::string_view &s) { return s == "one"; };

    // find latest element that satisfy the comparator, and projecting pair::first
    const auto subrange = std::ranges::find_last_if(list, cmp_one, &P::first);

    // print the found element and the "tail" after it
    for (P const& e : subrange)
        std::cout << '{' << std::quoted(e.first) << ", " << e.second << "} ";
    std::cout << '\n';
}
```

Output:

```text
{"one", 4} {"two", 5} {"three", 6}
```

### See also

- **ranges::find_end (C++20)** — finds the last sequence of elements in a
  certain range (niebloid)
- **ranges::findranges::find_ifranges::find_if_not (C++20)(C++20)(C++20)** —
  finds the first element satisfying specific criteria (niebloid)
- **ranges::search (C++20)** — searches for a range of elements (niebloid)
- **ranges::includes (C++20)** — returns `true` if one sequence is a subsequence
  of another (niebloid)
- **ranges::binary_search (C++20)** — determines if an element exists in a
  partially-ordered range (niebloid)
- **ranges::containsranges::contains_subrange (C++23)(C++23)** — checks if the
  range contains the given element or subrange (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/find_last*
