# std::ranges::advance

```cpp
Call signature
template< std::input_or_output_iterator I >
constexpr void advance( I& i, std::iter_difference_t<I> n );  // (1) (since C++20)
template< std::input_or_output_iterator I, std::sentinel_for<I> S >
constexpr void advance( I& i, S bound );  // (2) (since C++20)
template< std::input_or_output_iterator I, std::sentinel_for<I> S >
constexpr std::iter_difference_t<I> advance( I& i, std::iter_difference_t<I> n, S bound );  // (3) (since C++20)
```

1) Increments given iterator `i` for `n` times.

2) Increments given iterator `i` until `i == bound`.

3) Increments given iterator `i` for `n` times, or until `i == bound`, whichever
   comes first.

If `n` is negative, the iterator is decremented. In this case, `I` must model
`std::bidirectional_iterator`, and `S` must be the same type as `I` if `bound`
is provided, otherwise the behavior is undefined.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **i** — iterator to be advanced
- **bound** — sentinel denoting the end of the range `i` is an iterator to
- **n** — number of maximal increments of `i`

### Return value

3) The difference between `n` and the actual distance `i` traversed.

### Complexity

Linear.

However, if `I` additionally models `std::random_access_iterator`, or `S` models
`std::sized_sentinel_for<I>`, or `I` and `S` model `std::assignable_from<I&,
S>`, complexity is constant.

### Notes

The behavior is undefined if the specified sequence of increments or decrements
would require that a non-incrementable iterator (such as the past-the-end
iterator) is incremented, or that a non-decrementable iterator (such as the
front iterator or the singular iterator) is decremented.

### Possible implementation

```cpp
struct advance_fn
{
    template<std::input_or_output_iterator I>
    constexpr void operator()(I& i, std::iter_difference_t<I> n) const
    {
        if constexpr (std::random_access_iterator<I>)
            i += n;
        else
        {
            while (n > 0)
            {
                --n;
                ++i;
            }

            if constexpr (std::bidirectional_iterator<I>)
            {
                while (n < 0)
                {
                    ++n;
                    --i;
                }
            }
        }
    }

    template<std::input_or_output_iterator I, std::sentinel_for<I> S>
    constexpr void operator()(I& i, S bound) const
    {
        if constexpr (std::assignable_from<I&, S>)
            i = std::move(bound);
        else if constexpr (std::sized_sentinel_for<S, I>)
            (*this)(i, bound - i);
        else
            while (i != bound)
                ++i;
    }

    template<std::input_or_output_iterator I, std::sentinel_for<I> S>
    constexpr std::iter_difference_t<I>
    operator()(I& i, std::iter_difference_t<I> n, S bound) const
    {
        if constexpr (std::sized_sentinel_for<S, I>)
        {
            // std::abs is not constexpr until C++23
            auto abs = [](const std::iter_difference_t<I> x) { return x < 0 ? -x : x; };

            if (const auto dist = abs(n) - abs(bound - i); dist < 0)
            {
                (*this)(i, bound);
                return -dist;
            }

            (*this)(i, n);
            return 0;
        }
        else
        {
            while (n > 0 && i != bound)
            {
                --n;
                ++i;
            }

            if constexpr (std::bidirectional_iterator<I>)
            {
                while (n < 0 && i != bound)
                {
                    ++n;
                    --i;
                }
            }

            return n;
        }
    }
};

inline constexpr auto advance = advance_fn();
```

### Example

```cpp
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v {3, 1, 4};

    auto vi = v.begin();

    std::ranges::advance(vi, 2);
    std::cout << "1) value: " << *vi << '\n' << std::boolalpha;

    std::ranges::advance(vi, v.end());
    std::cout << "2) vi == v.end(): " << (vi == v.end()) << '\n';

    std::ranges::advance(vi, -3);
    std::cout << "3) value: " << *vi << '\n';

    std::cout << "4) diff: " << std::ranges::advance(vi, 2, v.end())
              << ", value: " << *vi << '\n';

    std::cout << "5) diff: " << std::ranges::advance(vi, 4, v.end())
              << ", vi == v.end(): " << (vi == v.end()) << '\n';
}
```

Output:

```text
1) value: 4
2) vi == v.end(): true
3) value: 3
4) diff: 0, value: 4
5) diff: 3, vi == v.end(): true
```

### See also

- **ranges::next (C++20)** — increment an iterator by a given distance or to a
  bound (niebloid)
- **ranges::prev (C++20)** — decrement an iterator by a given distance or to a
  bound (niebloid)
- **ranges::distance (C++20)** — returns the distance between an iterator and a
  sentinel, or between the beginning and end of a range (niebloid)
- **advance** — advances an iterator by given distance (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/ranges/advance*
