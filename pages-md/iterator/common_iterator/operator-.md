# operator-(std::common_iterator)

```cpp
template< std::sized_sentinel_for<I> I2, std::sized_sentinel_for<I> S2 >
    requires std::sized_sentinel_for<S, I2>
friend constexpr std::iter_difference_t<I2>
    operator-( const common_iterator& x, const std::common_iterator<I2, S2>& y );  // (since C++20)
```

Computes the distance between two iterator adaptors. Two sentinels are
considered equal.

Let `var` denote the underlying `std::variant` member object in
`std::common_iterator`, the behavior is undefined if either `x` or `y` is
invalid, i.e. `x.var.valueless_by_exception() || y.var.valueless_by_exception()`
is `true`.

This function template is not visible to ordinary unqualified or qualified
lookup, and can only be found by argument-dependent lookup when
`std::common_iterator<I>` is an associated class of the arguments.

### Parameters

- **x, y** — iterator adaptors to compute the difference of

### Return value

- `​0​` if `x.var` holds an `S` object and `y.var` holds an `S2` object, i.e.
  both of them hold a sentinel.
- Otherwise, `alt_x - alt_y`, where `alt_x` and `alt_y` are the alternatives
  held by `x.var` and `y.var`, respectively (either two iterators or one
  iterator and one sentinel).

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>

int main()
{
    int a[]{0, 1, 2, 3, 4, 5};

    using CI = std::common_iterator<
                   std::counted_iterator<int*>,
                   std::default_sentinel_t
                   >;

    CI i1{std::counted_iterator{a + 1, 2}};
    CI i2{std::counted_iterator{a, 3}};
    CI s1{std::default_sentinel};
    CI s2{std::default_sentinel};

    std::cout << (s2 - s1) << ' '
              << (i2 - i1) << ' '
              << (i1 - s1) << '\n';
}
```

Output:

```text
0 -1 -2
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3574 | C++20 | `variant` was fully constexpr (P2231R1) but
      `common_iterator` was not | also made constexpr

### See also

- **operator++operator++(int) (C++20)** — advances the iterator adaptor (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/iterator/common_iterator/operator-*
