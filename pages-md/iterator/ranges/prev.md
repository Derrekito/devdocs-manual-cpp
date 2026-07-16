# std::ranges::prev

```cpp
Call signature
template< std::bidirectional_iterator I >
constexpr I prev( I i );  // (1) (since C++20)
template< std::bidirectional_iterator I >
constexpr I prev( I i, std::iter_difference_t<I> n );  // (2) (since C++20)
template< std::bidirectional_iterator I >
constexpr I prev( I i, std::iter_difference_t<I> n, I bound );  // (3) (since C++20)
```

Return the `n`th predecessor of iterator `i`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **i** — an iterator
- **n** — number of elements `i` should be descended
- **bound** — iterator denoting the beginning of the range `i` points to

### Return value

1) The predecessor of `i`.

2) The `n`th predecessor of iterator `i`.

3) The `n`th predecessor of iterator `i`, or the first iterator that compares
   equal to `bound`, whichever is first.

### Complexity

1) Constant.

2,3) Constant if `I` models `std::random_access_iterator<I>`; otherwise linear.

### Possible implementation

```cpp
struct prev_fn
{
    template<std::bidirectional_iterator I>
    constexpr I operator()(I i) const
    {
        --i;
        return i;
    }

    template<std::bidirectional_iterator I>
    constexpr I operator()(I i, std::iter_difference_t<I> n) const
    {
        ranges::advance(i, -n);
        return i;
    }

    template<std::bidirectional_iterator I>
    constexpr I operator()(I i, std::iter_difference_t<I> n, I bound) const
    {
        ranges::advance(i, -n, bound);
        return i;
    }
};

inline constexpr auto prev = prev_fn();
```

### Notes

Although the expression `--r.end()` often compiles for containers, it is not
guaranteed to do so: `r.end()` is an rvalue expression, and there is no iterator
requirement that specifies that decrement of an rvalue is guaranteed to work. In
particular, when iterators are implemented as pointers or its `operator--` is
lvalue-ref-qualified, `--r.end()` does not compile, while
`ranges::prev(r.end())` does.

This is further exacerbated by ranges that do not model `ranges::common_range`.
For example, for some underlying ranges, `ranges::transform_view::end` doesn't
have the same return type as `ranges::transform_view::begin`, and so `--r.end()`
won't compile. This isn't something that `ranges::prev` can aid with, but there
are workarounds.

### Example

```cpp
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v{3, 1, 4};
    auto pv = std::ranges::prev(v.end(), 2);
    std::cout << *pv << '\n';

    pv = std::ranges::prev(pv, 42, v.begin());
    std::cout << *pv << '\n';
}
```

Output:

```text
1
3
```

### See also

- **ranges::next (C++20)** — increment an iterator by a given distance or to a
  bound (niebloid)
- **ranges::advance (C++20)** — advances an iterator by given distance or to a
  given bound (niebloid)
- **prev (C++11)** — decrement an iterator (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/ranges/prev*
