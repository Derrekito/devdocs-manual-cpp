# std::ranges::prev_permutation, std::ranges::prev_permutation_result

```cpp
Call signature
template< std::bidirectional_iterator I, std::sentinel_for<I> S,
          class Comp = ranges::less, class Proj = std::identity >
requires std::sortable<I, Comp, Proj>
constexpr prev_permutation_result<I>
    prev_permutation( I first, S last, Comp comp = {}, Proj proj = {} );  // (1) (since C++20)
template< ranges::bidirectional_range R, class Comp = ranges::less,
          class Proj = std::identity >
requires std::sortable<ranges::iterator_t<R>, Comp, Proj>
constexpr prev_permutation_result<ranges::borrowed_iterator_t<R>>
    prev_permutation( R&& r, Comp comp = {}, Proj proj = {} );  // (2) (since C++20)
Helper type
template< class I >
using prev_permutation_result = ranges::in_found_result<I>;  // (3) (since C++20)
```

1) Transforms the range `[``first``,``last``)` into the previous permutation,
   where the set of all permutations is ordered lexicographically with respect
   to binary comparison function object `comp` and projection function object
   `proj`.
Returns:

- `{last, true}` if "previous" permutation exists. Otherwise,
- `{last, false}`, and transforms the range into the (lexicographically) last
  permutation, as if by

```cpp
ranges::sort(first, last, comp, proj);
ranges::reverse(first, last);
```

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

- **first, last** — the range of elements to "permute"
- **r** — the range of elements to "permute"
- **comp** — comparison function object which returns `true` if the first
  argument is less than the second
- **proj** — projection to apply to the elements

### Return value

1) `ranges::prev_permutation_result<I>{last, true}` if the new permutation is
   lexicographically less than the old one.
   `ranges::prev_permutation_result<I>{last, false}` if the first permutation
   was reached and the range was reset to the last permutation.

2) Same as (1) except that the return type is
   `ranges::prev_permutation_result<ranges::borrowed_iterator_t<R>>`.

### Exceptions

Any exceptions thrown from iterator operations or the element swap.

### Complexity

At most \(\scriptsize N/2\)N / 2 swaps, where \(\scriptsize N\)N is
`ranges::distance(first, last)` in case (1) or `ranges::distance(r)` in case
(2). Averaged over the entire sequence of permutations, typical implementations
use about 3 comparisons and 1.5 swaps per call.

### Notes

Implementations (e.g. MSVC STL) may enable vectorization when the iterator type
models `contiguous_iterator` and swapping its value type calls neither
non-trivial special member function nor ADL-found `swap`.

### Possible implementation

```cpp
struct prev_permutation_fn
{
    template<std::bidirectional_iterator I, std::sentinel_for<I> S,
             class Comp = ranges::less, class Proj = std::identity>
    requires std::sortable<I, Comp, Proj>
    constexpr ranges::prev_permutation_result<I>
        operator()(I first, S last, Comp comp = {}, Proj proj = {}) const
    {
        // check that the sequence has at least two elements
        if (first == last)
            return {std::move(first), false};
        auto i{first};
        ++i;
        if (i == last)
            return {std::move(i), false};
        auto i_last{ranges::next(first, last)};
        i = i_last;
        --i;
        // main "permutating" loop
        for (;;)
        {
            auto i1{i};
            --i;
            if (std::invoke(comp, std::invoke(proj, *i1), std::invoke(proj, *i)))
            {
                auto j{i_last};
                while (!std::invoke(comp, std::invoke(proj, *--j), std::invoke(proj, *i)))
                    ;
                ranges::iter_swap(i, j);
                ranges::reverse(i1, last);
                return {std::move(i_last), true};
            }
            // permutation "space" is exhausted
            if (i == first)
            {
                ranges::reverse(first, last);
                return {std::move(i_last), false};
            }
        }
    }

    template<ranges::bidirectional_range R, class Comp = ranges::less,
             class Proj = std::identity>
    requires std::sortable<ranges::iterator_t<R>, Comp, Proj>
    constexpr ranges::prev_permutation_result<ranges::borrowed_iterator_t<R>>
        operator()(R&& r, Comp comp = {}, Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r),
                       std::move(comp), std::move(proj));
    }
};

inline constexpr prev_permutation_fn prev_permutation {};
```

### Example

```cpp
#include <algorithm>
#include <array>
#include <compare>
#include <functional>
#include <iostream>
#include <string>

struct S
{
    char c{};
    int i{};
    auto operator<=>(const S&) const = default;
    friend std::ostream& operator<<(std::ostream& os, const S& s)
    {
        return os << "{'" << s.c << "', " << s.i << "}";
    }
};

auto print = [](auto const& v, char term = ' ')
{
    std::cout << "{ ";
    for (const auto& e : v)
        std::cout << e << ' ';
    std::cout << '}' << term;
};

int main()
{
    std::cout << "Generate all permutations (iterators case):\n";
    std::string s{"cba"};
    do print(s);
    while (std::ranges::prev_permutation(s.begin(), s.end()).found);

    std::cout << "\nGenerate all permutations (range case):\n";
    std::array a{'c', 'b', 'a'};
    do print(a);
    while (std::ranges::prev_permutation(a).found);

    std::cout << "\nGenerate all permutations using comparator:\n";
    using namespace std::literals;
    std::array z{"▁"s, "▄"s, "█"s};
    do print(z);
    while (std::ranges::prev_permutation(z, std::greater()).found);

    std::cout << "\nGenerate all permutations using projection:\n";
    std::array<S, 3> r{S{'C',1}, S{'B',2}, S{'A',3}};
    do print(r, '\n');
    while (std::ranges::prev_permutation(r, {}, &S::c).found);
}
```

Output:

```text
Generate all permutations (iterators case):
{ c b a } { c a b } { b c a } { b a c } { a c b } { a b c }
Generate all permutations (range case):
{ c b a } { c a b } { b c a } { b a c } { a c b } { a b c }
Generate all permutations using comparator:
{ ▁ ▄ █ } { ▁ █ ▄ } { ▄ ▁ █ } { ▄ █ ▁ } { █ ▁ ▄ } { █ ▄ ▁ }
Generate all permutations using projection:
{ {'C', 1} {'B', 2} {'A', 3} }
{ {'C', 1} {'A', 3} {'B', 2} }
{ {'B', 2} {'C', 1} {'A', 3} }
{ {'B', 2} {'A', 3} {'C', 1} }
{ {'A', 3} {'C', 1} {'B', 2} }
{ {'A', 3} {'B', 2} {'C', 1} }
```

### See also

- **ranges::next_permutation (C++20)** — generates the next greater
  lexicographic permutation of a range of elements (niebloid)
- **ranges::is_permutation (C++20)** — determines if a sequence is a permutation
  of another sequence (niebloid)
- **next_permutation** — generates the next greater lexicographic permutation of
  a range of elements (function template)
- **prev_permutation** — generates the next smaller lexicographic permutation of
  a range of elements (function template)
- **is_permutation (C++11)** — determines if a sequence is a permutation of
  another sequence (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/prev_permutation*
