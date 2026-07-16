# std::ranges::find, std::ranges::find_if, std::ranges::find_if_not

```cpp
Call signature
template< std::input_iterator I, std::sentinel_for<I> S,
          class T, class Proj = std::identity >
requires std::indirect_binary_predicate<ranges::equal_to, std::projected<I, Proj>,
                                        const T*>
constexpr I
    find( I first, S last, const T& value, Proj proj = {} );  // (1) (since C++20)
template< ranges::input_range R, class T, class Proj = std::identity >
requires std::indirect_binary_predicate<ranges::equal_to,
                                        std::projected<ranges::iterator_t<R>, Proj>,
                                        const T*>
constexpr ranges::borrowed_iterator_t<R>
    find( R&& r, const T& value, Proj proj = {} );  // (2) (since C++20)
template< std::input_iterator I, std::sentinel_for<I> S,
          class Proj = std::identity,
          std::indirect_unary_predicate<std::projected<I, Proj>> Pred >
constexpr I
    find_if( I first, S last, Pred pred, Proj proj = {} );  // (3) (since C++20)
template< ranges::input_range R, class Proj = std::identity,
          std::indirect_unary_predicate<std::projected<ranges::iterator_t<R>,
                                        Proj>> Pred >
constexpr ranges::borrowed_iterator_t<R>
    find_if( R&& r, Pred pred, Proj proj = {} );  // (4) (since C++20)
template< std::input_iterator I, std::sentinel_for<I> S,
          class Proj = std::identity,
          std::indirect_unary_predicate<std::projected<I, Proj>> Pred >
constexpr I
    find_if_not( I first, S last, Pred pred, Proj proj = {} );  // (5) (since C++20)
template< ranges::input_range R, class Proj = std::identity,
          std::indirect_unary_predicate<std::projected<ranges::iterator_t<R>,
                                        Proj>> Pred >
constexpr ranges::borrowed_iterator_t<R>
    find_if_not( R&& r, Pred pred, Proj proj = {} );  // (6) (since C++20)
```

Returns the first element in the range `[``first``,``last``)` that satisfies
specific criteria:

1) `find` searches for an element equal to `value`.

3) `find_if` searches for an element for which predicate `pred` returns `true`.

5) `find_if_not` searches for an element for which predicate `pred` returns
   `false`.

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

Iterator to the first element satisfying the condition or iterator equal to
`last` if no such element is found.

### Complexity

At most `last - first` applications of the predicate and projection.

### Possible implementation

```cpp
struct find_fn
{
    template<std::input_iterator I, std::sentinel_for<I> S,
             class T, class Proj = std::identity>
    requires std::indirect_binary_predicate<
                 ranges::equal_to, std::projected<I, Proj>, const T*>
    constexpr I operator()(I first, S last, const T& value, Proj proj = {}) const
    {
        for (; first != last; ++first)
            if (std::invoke(proj, *first) == value)
                return first;
        return first;
    }

    template<ranges::input_range R, class T, class Proj = std::identity>
    requires std::indirect_binary_predicate<ranges::equal_to,
                 std::projected<ranges::iterator_t<R>, Proj>, const T*>
    constexpr ranges::borrowed_iterator_t<R>
        operator()(R&& r, const T& value, Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), value, std::ref(proj));
    }
};

inline constexpr find_fn find;
```

```cpp
struct find_if_fn
{
    template<std::input_iterator I, std::sentinel_for<I> S, class Proj = std::identity,
             std::indirect_unary_predicate<std::projected<I, Proj>> Pred>
    constexpr I operator()(I first, S last, Pred pred, Proj proj = {}) const
    {
        for (; first != last; ++first)
            if (std::invoke(pred, std::invoke(proj, *first)))
                return first;
        return first;
    }

    template<ranges::input_range R, class Proj = std::identity,
             std::indirect_unary_predicate<
                 std::projected<ranges::iterator_t<R>, Proj>> Pred>
    constexpr ranges::borrowed_iterator_t<R>
        operator()(R&& r, Pred pred, Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), std::ref(pred), std::ref(proj));
    }
};

inline constexpr find_if_fn find_if;
```

```cpp
struct find_if_not_fn
{
    template<std::input_iterator I, std::sentinel_for<I> S, class Proj = std::identity,
             std::indirect_unary_predicate<std::projected<I, Proj>> Pred>
    constexpr I operator()(I first, S last, Pred pred, Proj proj = {}) const
    {
        for (; first != last; ++first)
            if (!std::invoke(pred, std::invoke(proj, *first)))
                return first;
        return first;
    }

    template<ranges::input_range R, class Proj = std::identity,
             std::indirect_unary_predicate<
                 std::projected<ranges::iterator_t<R>, Proj>> Pred>
    constexpr ranges::borrowed_iterator_t<R>
        operator()(R&& r, Pred pred, Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), std::ref(pred), std::ref(proj));
    }
};

inline constexpr find_if_not_fn find_if_not;
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>

int main()
{
    namespace ranges = std::ranges;

    const int n1 = 3;
    const int n2 = 5;
    const auto v = {4, 1, 3, 2};

    if (ranges::find(v, n1) != v.end())
        std::cout << "v contains: " << n1 << '\n';
    else
        std::cout << "v does not contain: " << n1 << '\n';

    if (ranges::find(v.begin(), v.end(), n2) != v.end())
        std::cout << "v contains: " << n2 << '\n';
    else
        std::cout << "v does not contain: " << n2 << '\n';

    auto is_even = [](int x) { return x % 2 == 0; };

    if (auto result = ranges::find_if(v.begin(), v.end(), is_even); result != v.end())
        std::cout << "First even element in v: " << *result << '\n';
    else
        std::cout << "No even elements in v\n";

    if (auto result = ranges::find_if_not(v, is_even); result != v.end())
        std::cout << "First odd element in v: " << *result << '\n';
    else
        std::cout << "No odd elements in v\n";

    auto divides_13 = [](int x) { return x % 13 == 0; };

    if (auto result = ranges::find_if(v, divides_13); result != v.end())
        std::cout << "First element divisible by 13 in v: " << *result << '\n';
    else
        std::cout << "No elements in v are divisible by 13\n";

    if (auto result = ranges::find_if_not(v.begin(), v.end(), divides_13);
        result != v.end())
        std::cout << "First element indivisible by 13 in v: " << *result << '\n';
    else
        std::cout << "All elements in v are divisible by 13\n";
}
```

Output:

```text
v contains: 3
v does not contain: 5
First even element in v: 4
First odd element in v: 1
No elements in v are divisible by 13
First element indivisible by 13 in v: 4
```

### See also

- **ranges::adjacent_find (C++20)** — finds the first two adjacent items that
  are equal (or satisfy a given predicate) (niebloid)
- **ranges::find_end (C++20)** — finds the last sequence of elements in a
  certain range (niebloid)
- **ranges::find_first_of (C++20)** — searches for any one of a set of elements
  (niebloid)
- **ranges::mismatch (C++20)** — finds the first position where two ranges
  differ (niebloid)
- **ranges::search (C++20)** — searches for a range of elements (niebloid)
- **findfind_iffind_if_not (C++11)** — finds the first element satisfying
  specific criteria (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/find*
