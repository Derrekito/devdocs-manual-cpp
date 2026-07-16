# std::ranges::next

```cpp
Call signature
template< std::input_or_output_iterator I >
constexpr I next( I i );  // (1) (since C++20)
template< std::input_or_output_iterator I >
constexpr I next( I i, std::iter_difference_t<I> n );  // (2) (since C++20)
template< std::input_or_output_iterator I, std::sentinel_for<I> S >
constexpr I next( I i, S bound );  // (3) (since C++20)
template< std::input_or_output_iterator I, std::sentinel_for<I> S >
constexpr I next( I i, std::iter_difference_t<I> n, S bound );  // (4) (since C++20)
```

Return the `n`th successor of iterator `i`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **i** — an iterator
- **n** — number of elements to advance
- **bound** — sentinel denoting the end of the range `i` points to

### Return value

1) The successor of iterator `i`.

2) The `n`th successor of iterator `i`.

3) The first iterator equivalent to `bound`.

4) The `n`th successor of iterator `i`, or the first iterator equivalent to
   `bound`, whichever is first.

### Complexity

1) Constant.

2) Constant if `I` models `std::random_access_iterator`; otherwise linear.

3) Constant if `I` and `S` models both `std::random_access_iterator<I>` and
   `std::sized_sentinel_for<S, I>`, or if `I` and `S` models
   `std::assignable_from<I&, S>`; otherwise linear.

4) Constant if `I` and `S` models both `std::random_access_iterator<I>` and
   `std::sized_sentinel_for<S, I>`; otherwise linear.

### Possible implementation

```cpp
struct next_fn
{
    template<std::input_or_output_iterator I>
    constexpr I operator()(I i) const
    {
        ++i;
        return i;
    }

    template<std::input_or_output_iterator I>
    constexpr I operator()(I i, std::iter_difference_t<I> n) const
    {
        ranges::advance(i, n);
        return i;
    }

    template<std::input_or_output_iterator I, std::sentinel_for<I> S>
    constexpr I operator()(I i, S bound) const
    {
        ranges::advance(i, bound);
        return i;
    }

    template<std::input_or_output_iterator I, std::sentinel_for<I> S>
    constexpr I operator()(I i, std::iter_difference_t<I> n, S bound) const
    {
        ranges::advance(i, n, bound);
        return i;
    }
};

inline constexpr auto next = next_fn();
```

### Notes

Although the expression `++x.begin()` often compiles, it is not guaranteed to do
so: `x.begin()` is an rvalue expression, and there is no requirement that
specifies that increment of an rvalue is guaranteed to work. In particular, when
iterators are implemented as pointers or its `operator++` is
lvalue-ref-qualified, `++x.begin()` does not compile, while
`ranges::next(x.begin())` does.

### Example

```cpp
#include <cassert>
#include <iterator>

int main()
{
    auto v = {3, 1, 4};
    {
        auto n = std::ranges::next(v.begin());
        assert(*n == 1);
    }
    {
        auto n = std::ranges::next(v.begin(), 2);
        assert(*n == 4);
    }
    {
        auto n = std::ranges::next(v.begin(), v.end());
        assert(n == v.end());
    }
    {
        auto n = std::ranges::next(v.begin(), 42, v.end());
        assert(n == v.end());
    }
}
```

### See also

- **ranges::prev (C++20)** — decrement an iterator by a given distance or to a
  bound (niebloid)
- **ranges::advance (C++20)** — advances an iterator by given distance or to a
  given bound (niebloid)
- **next (C++11)** — increment an iterator (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/ranges/next*
