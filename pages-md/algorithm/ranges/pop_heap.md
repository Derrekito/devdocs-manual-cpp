# std::ranges::pop_heap

```cpp
Call signature
template< std::random_access_iterator I, std::sentinel_for<I> S,
          class Comp = ranges::less, class Proj = std::identity >
requires std::sortable<I, Comp, Proj>
constexpr I
    pop_heap( I first, S last, Comp comp = {}, Proj proj = {} );  // (1) (since C++20)
template< ranges::random_access_range R, class Comp = ranges::less,
          class Proj = std::identity >
requires std::sortable<ranges::iterator_t<R>, Comp, Proj>
constexpr ranges::borrowed_iterator_t<R>
    pop_heap( R&& r, Comp comp = {}, Proj proj = {} );  // (2) (since C++20)
```

Swaps the value in the position `first` and the value in the position `last - 1`
and makes the subrange `[``first``,``last - 1``)` into a max heap. This has the
effect of removing the first element from the heap defined by the range
`[``first``,``last``)`.

1) Elements are compared using the given binary comparison function `comp` and
   projection object `proj`.

2) Same as (1), but uses `r` as the range, as if using `ranges::begin(r)` as
   `first` and `ranges::end(r)` as `last`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first, last** — the range of elements defining the valid nonempty heap to
  modify
- **r** — the range of elements defining the valid nonempty heap to modify
- **pred** — predicate to apply to the projected elements
- **proj** — projection to apply to the elements

### Return value

An iterator equal to `last`.

### Complexity

Given `N = ranges::distance(first, last)`, at most \(\scriptsize
2\log{(N)}\)2log(N) comparisons and \(\scriptsize 4\log{(N)}\)4log(N)
projections.

### Notes

A *max heap* is a range of elements `[``f``,``l``)`, arranged with respect to
comparator `comp` and projection `proj`, that has the following properties:

- With `N = l - f`, `p = f[(i - 1) / 2]`, and `q = f[i]`, for all `0 < i < N`,
  the expression `std::invoke(comp, std::invoke(proj, p), std::invoke(proj, q))`
  evaluates to `false`.
- A new element can be added using `ranges::push_heap`, in \(\scriptsize
  \mathcal{O}(\log N)\)𝓞(log N) time.
- The first element can be removed using `ranges::pop_heap`, in \(\scriptsize
  \mathcal{O}(\log N)\)𝓞(log N) time.

### Example

```cpp
#include <algorithm>
#include <array>
#include <iostream>
#include <iterator>
#include <string_view>

template<class I = int*>
void print(std::string_view rem, I first = {}, I last = {},
           std::string_view term = "\n")
{
    for (std::cout << rem; first != last; ++first)
        std::cout << *first << ' ';
    std::cout << term;
}

int main()
{
    std::array v {3, 1, 4, 1, 5, 9, 2, 6, 5, 3};
    print("initially, v: ", v.cbegin(), v.cend());

    std::ranges::make_heap(v);
    print("make_heap, v: ", v.cbegin(), v.cend());

    print("convert heap into sorted array:");
    for (auto n {std::ssize(v)}; n >= 0; --n)
    {
        std::ranges::pop_heap(v.begin(), v.begin() + n);
        print("[ ", v.cbegin(), v.cbegin() + n, "]  ");
        print("[ ", v.cbegin() + n, v.cend(), "]\n");
    }
}
```

Output:

```text
initially, v: 3 1 4 1 5 9 2 6 5 3
make_heap, v: 9 6 4 5 5 3 2 1 1 3
convert heap into sorted array:
[ 6 5 4 3 5 3 2 1 1 9 ]  [ ]
[ 5 5 4 3 1 3 2 1 6 ]  [ 9 ]
[ 5 3 4 1 1 3 2 5 ]  [ 6 9 ]
[ 4 3 3 1 1 2 5 ]  [ 5 6 9 ]
[ 3 2 3 1 1 4 ]  [ 5 5 6 9 ]
[ 3 2 1 1 3 ]  [ 4 5 5 6 9 ]
[ 2 1 1 3 ]  [ 3 4 5 5 6 9 ]
[ 1 1 2 ]  [ 3 3 4 5 5 6 9 ]
[ 1 1 ]  [ 2 3 3 4 5 5 6 9 ]
[ 1 ]  [ 1 2 3 3 4 5 5 6 9 ]
[ ]  [ 1 1 2 3 3 4 5 5 6 9 ]
```

### See also

- **ranges::push_heap (C++20)** — adds an element to a max heap (niebloid)
- **ranges::is_heap (C++20)** — checks if the given range is a max heap
  (niebloid)
- **ranges::is_heap_until (C++20)** — finds the largest subrange that is a max
  heap (niebloid)
- **ranges::make_heap (C++20)** — creates a max heap out of a range of elements
  (niebloid)
- **ranges::sort_heap (C++20)** — turns a max heap into a range of elements
  sorted in ascending order (niebloid)
- **pop_heap** — removes the largest element from a max heap (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/pop_heap*
