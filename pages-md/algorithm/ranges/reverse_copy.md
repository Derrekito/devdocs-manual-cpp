# std::ranges::reverse_copy, std::ranges::reverse_copy_result

```cpp
Call signature
template< std::bidirectional_iterator I, std::sentinel_for<I> S,
          std::weakly_incrementable O >
requires std::indirectly_copyable<I, O>
constexpr reverse_copy_result<I, O>
    reverse_copy( I first, S last, O result );  // (1) (since C++20)
template< ranges::bidirectional_range R, std::weakly_incrementable O >
requires std::indirectly_copyable<ranges::iterator_t<R>, O>
constexpr reverse_copy_result<ranges::borrowed_iterator_t<R>, O>
    reverse_copy( R&& r, O result );  // (2) (since C++20)
Helper types
template< class I, class O >
using reverse_copy_result = ranges::in_out_result<I, O>;  // (3) (since C++20)
```

1) Copies the elements from the source range `[``first``,``last``)` to the
   destination range `[``result``,``result + N``)`, where `N` is
   `ranges::distance(first, last)`, in such a way that the elements in the new
   range are in reverse order. Behaves as if by executing the assignment
   `*(result + N - 1 - i) = *(first + i)` once for each integer `i` in
   `[``​0​``,``N``)`. The behavior is undefined if the source and destination
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

- **first, last** — the range of elements to copy
- **r** — the range of elements to copy
- **result** — the beginning of the destination range.

### Return value

`{last, result + N}`.

### Complexity

Exactly `N` assignments.

### Notes

Implementations (e.g. MSVC STL) may enable vectorization when the both iterator
types model `contiguous_iterator` and have the same value type, and the value
type is TriviallyCopyable.

### Possible implementation

See also the implementations in MSVC STL and libstdc++.

```cpp
struct reverse_copy_fn
{
    template<std::bidirectional_iterator I, std::sentinel_for<I> S,
             std::weakly_incrementable O>
    requires std::indirectly_copyable<I, O>
    constexpr ranges::reverse_copy_result<I, O>
        operator()(I first, S last, O result) const
    {
        auto ret = ranges::next(first, last);
        for (; last != first; *result = *--last, ++result);
        return {std::move(ret), std::move(result)};
    }

    template<ranges::bidirectional_range R, std::weakly_incrementable O>
    requires std::indirectly_copyable<ranges::iterator_t<R>, O>
    constexpr ranges::reverse_copy_result<ranges::borrowed_iterator_t<R>, O>
        operator()(R&& r, O result) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), std::move(result));
    }
};

inline constexpr reverse_copy_fn reverse_copy {};
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <string>

int main()
{
    std::string x {"12345"}, y(x.size(), ' ');
    std::cout << x << " → ";
    std::ranges::reverse_copy(x.begin(), x.end(), y.begin());
    std::cout << y << " → ";
    std::ranges::reverse_copy(y, x.begin());
    std::cout << x << '\n';
}
```

Output:

```text
12345 → 54321 → 12345
```

### See also

- **ranges::reverse (C++20)** — reverses the order of elements in a range
  (niebloid)
- **reverse_copy** — creates a copy of a range that is reversed (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/reverse_copy*
