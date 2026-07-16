# operator==,<=>(std::counted_iterator)

```cpp
template< std::common_with<I> I2 >
    friend constexpr bool operator==(
        const counted_iterator& x, const counted_iterator<I2>& y );  // (1) (since C++20)
template< std::common_with<I> I2 >
    friend constexpr strong_ordering operator<=>(
        const counted_iterator& x, const counted_iterator<I2>& y );  // (2) (since C++20)
```

Compares the underlying lengths (i.e. distances to the end).

1) Checks if the underlying lengths are equal.

2) Compares the underlying lengths with operator `<=>`.

The behavior is undefined if `x` and `y` do not point to elements of the same
sequence. That is, there must exist some `n` such that `std::next(x.base(),
x.count() + n)` and `std::next(y.base(), y.count() + n)` refer to the same
element.

The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.

This function template is not visible to ordinary unqualified or qualified
lookup, and can only be found by argument-dependent lookup when
`std::counted_iterator<I>` is an associated class of the arguments.

### Parameters

- **x, y** — iterator adaptors

### Return value

1) `x.count() == y.count()`

2) `y.count() <=> x.count()`

### Notes

Since the *length* counts down, not up, the order of the arguments of
`operator<=>` in the underlying comparison expression is reversed, i.e. `y` is
*lhs*, `x` is *rhs*.

### Example

```cpp
#include <initializer_list>
#include <iterator>

int main()
{
    static constexpr auto v = {1, 2, 3, 4, 5, 6};
    constexpr std::counted_iterator<std::initializer_list<int>::iterator>
        it1{v.begin(), 5},
        it2{v.begin(), 5},
        it3{v.begin() + 1, 4},
        it4{v.begin(), 0};
    static_assert(it1 == it2);
    static_assert(it2 != it3);
    static_assert(it2 <  it3);
    static_assert(it1 <= it2);
    static_assert(it3 != std::default_sentinel);
    static_assert(it4 == std::default_sentinel);

//  it2 == std::counted_iterator{v.begin(), 4}; // UB: operands do not refer to
                                                // elements of the same sequence
}
```

### See also

- **operator==(std::default_sentinel) (C++20)** — checks if the distance to the
  end is equal to `​0​` (function template)
- **operator+ (C++20)** — advances the iterator (function template)
- **operator- (C++20)** — computes the distance between two iterator adaptors
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/counted_iterator/operator_cmp*
