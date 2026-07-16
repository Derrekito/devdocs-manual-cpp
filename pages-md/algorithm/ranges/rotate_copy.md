# std::ranges::rotate_copy, std::ranges::rotate_copy_result

```cpp
Call signature
template< std::forward_iterator I, std::sentinel_for<I> S,
          std::weakly_incrementable O >
requires std::indirectly_copyable<I, O>
constexpr rotate_copy_result<I, O>
    rotate_copy( I first, I middle, S last, O result );  // (1) (since C++20)
template< ranges::forward_range R, std::weakly_incrementable O >
requires std::indirectly_copyable<ranges::iterator_t<R>, O>
constexpr rotate_copy_result<ranges::borrowed_iterator_t<R>, O>
    rotate_copy( R&& r, ranges::iterator_t<R> middle, O result );  // (2) (since C++20)
Helper types
template< class I, class O >
using rotate_copy_result = in_out_result<I, O>;  // (3) (since C++20)
```

1) Copies the elements from the source range `[``first``,``last``)`, to the
   destination range beginning at `result` in such a way, that the element
   `*middle` becomes the first element of the destination range and `*(middle -
   1)` becomes the last element. The result is that the destination range
   contains a *left rotated* copy of the source range.

The behavior is undefined if either `[``first``,``middle``)` or
   `[``middle``,``last``)` is not a valid range, or the source and destination
   ranges overlap.

2) Same as (1), but uses `r` as the source range, as if using `ranges::begin(r)`
   as `first` and `ranges::end(r)` as `last`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first, last** — the source range of elements to copy from
- **r** — the source range of elements to copy from
- **middle** — the iterator to the element that should appear at the beginning
  of the destination range
- **result** — beginning of the destination range

### Return value

`{last, result + N}`, where `N = ranges::distance(first, last)`.

### Complexity

*Linear*: exactly `N` assignments.

### Notes

If the value type is TriviallyCopyable and the iterator types satisfy
`contiguous_iterator`, implementations of `ranges::rotate_copy` usually avoid
multiple assignments by using a "bulk copy" function such as `std::memmove`.

### Possible implementation

See also the implementations in libstdc++ and MSVC STL.

```cpp
struct rotate_copy_fn
{
    template<std::forward_iterator I, std::sentinel_for<I> S, std::weakly_incrementable O>
    requires std::indirectly_copyable<I, O>
    constexpr ranges::rotate_copy_result<I, O>
        operator()(I first, I middle, S last, O result) const
    {
        auto c1 {ranges::copy(middle, std::move(last), std::move(result))};
        auto c2 {ranges::copy(std::move(first), std::move(middle), std::move(c1.out))};
        return {std::move(c1.in), std::move(c2.out)};
    }

    template<ranges::forward_range R, std::weakly_incrementable O>
    requires std::indirectly_copyable<ranges::iterator_t<R>, O>
    constexpr ranges::rotate_copy_result<ranges::borrowed_iterator_t<R>, O>
        operator()(R&& r, ranges::iterator_t<R> middle, O result) const
    {
        return (*this)(ranges::begin(r), std::move(middle),
                       ranges::end(r), std::move(result));
    }
};

inline constexpr rotate_copy_fn rotate_copy {};
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> src {1, 2, 3, 4, 5};
    std::vector<int> dest(src.size());
    auto pivot = std::ranges::find(src, 3);

    std::ranges::rotate_copy(src, pivot, dest.begin());
    for (int i : dest)
        std::cout << i << ' ';
    std::cout << '\n';

    // copy the rotation result directly to the std::cout
    pivot = std::ranges::find(dest, 1);
    std::ranges::rotate_copy(dest, pivot, std::ostream_iterator<int>(std::cout, " "));
    std::cout << '\n';
}
```

Output:

```text
3 4 5 1 2
1 2 3 4 5
```

### See also

- **ranges::rotate (C++20)** — rotates the order of elements in a range
  (niebloid)
- **ranges::copyranges::copy_if (C++20)(C++20)** — copies a range of elements to
  a new location (niebloid)
- **rotate_copy** — copies and rotate a range of elements (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/rotate_copy*
