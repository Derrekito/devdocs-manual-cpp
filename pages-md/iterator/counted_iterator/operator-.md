# operator-(std::counted_iterator)

```cpp
template< std::common_with<I> I2 >
    friend constexpr std::iter_difference_t<I2> operator-(
        const counted_iterator& x, const counted_iterator<I2>& y );  // (since C++20)
```

Computes the distance between two iterator adaptors.

The behavior is undefined if `x` and `y` do not point to elements of the same
sequence. That is, there must exist some `n` such that `std::next(x.base(),
x.count() + n)` and `std::next(y.base(), y.count() + n)` refer to the same
element.

This function template is not visible to ordinary unqualified or qualified
lookup, and can only be found by argument-dependent lookup when
`std::counted_iterator<I>` is an associated class of the arguments.

### Parameters

- **x, y** — iterator adaptors to compute the difference of

### Return value

`y.count() - x.count()`

### Notes

Since the *length* counts down, not up, the order of the arguments of
`operator-` in the underlying expression is reversed, i.e. `y` is *lhs* and `x`
is *rhs*.

### Example

```cpp
#include <initializer_list>
#include <iterator>

int main()
{
    static constexpr auto v = {1, 2, 3, 4, 5, 6};
    constexpr std::counted_iterator<std::initializer_list<int>::iterator>
        it1{v.begin(), 5},
        it2{it1 + 3},
        it3{v.begin(), 2};

    static_assert(it1 - it2 == -3);
    static_assert(it2 - it1 == +3);
//  static_assert(it1 - it3 == -3); // UB: operands of operator- do not refer to
                                    // elements of the same sequence
}
```

### See also

-
  **operator++operator++(int)operator+=operator+operator--operator--(int)operator-=operator-
  (C++20)** — advances or decrements the iterator (public member function)
- **operator+ (C++20)** — advances the iterator (function template)
- **operator-(std::default_sentinel_t) (C++20)** — computes the signed distance
  to the end (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/counted_iterator/operator-*
