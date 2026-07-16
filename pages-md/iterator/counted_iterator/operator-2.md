# operator-(std::counted_iterator<I>, std::default_sentinel_t)

```cpp
friend constexpr std::iter_difference_t<I> operator-(
    const counted_iterator& x, std::default_sentinel_t );  // (1) (since C++20)
friend constexpr std::iter_difference_t<I> operator-(
    std::default_sentinel_t, const counted_iterator& y );  // (2) (since C++20)
```

1) Returns the negative distance to the end.

2) Returns the positive distance to the end.

This function template is not visible to ordinary unqualified or qualified
lookup, and can only be found by argument-dependent lookup when
`std::counted_iterator<I>` is an associated class of the arguments.

### Parameters

- **x, y** — iterator adaptors to compute the difference of

### Return value

1) `-x.count()`

2) `y.count()`

### Example

```cpp
#include <initializer_list>
#include <iterator>

int main()
{
    constexpr static auto v = {1, 2, 3, 4};
    constexpr std::counted_iterator<std::initializer_list<int>::iterator>
        it{v.begin(), 3};
    constexpr auto d1 = it - std::default_sentinel;
    static_assert(d1 == -3); // (1)
    constexpr auto d2 = std::default_sentinel - it;
    static_assert(d2 == +3); // (2)
}
```

### See also

-
  **operator++operator++(int)operator+=operator+operator--operator--(int)operator-=operator-
  (C++20)** — advances or decrements the iterator (public member function)
- **operator+ (C++20)** — advances the iterator (function template)
- **operator- (C++20)** — computes the distance between two iterator adaptors
  (function template)
- **default_sentinel_t (C++20)** — default sentinel for use with iterators that
  know the bound of their range (class)

---
*Source: https://en.cppreference.com/w/cpp/iterator/counted_iterator/operator-2*
