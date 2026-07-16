# iter_swap(std::counted_iterator)

```cpp
template< std::indirectly_swappable<I> I2 >
    friend constexpr void
        iter_swap( const counted_iterator& x, const std::counted_iterator<I2>& y )
            noexcept(noexcept(ranges::iter_swap(x.base(), y.base())));  // (since C++20)
```

Swaps the objects pointed to by two underlying iterators. The behavior is
undefined if either `x.count()` or `y.count()` is equal to `​0​`.

The function body is equivalent to: `ranges::iter_swap(x.base(), y.base());`.

This function template is not visible to ordinary unqualified or qualified
lookup, and can only be found by argument-dependent lookup when
`std::counted_iterator<I>` is an associated class of the arguments.

### Parameters

- **x, y** — iterator adaptors to the elements to swap

### Return value

(none)

### Complexity

Constant.

### Example

```cpp
#include <iostream>
#include <iterator>
#include <list>
#include <vector>

int main()
{
    std::vector p{1, 2, 3, 4},
                q{5, 6, 7, 8};

    std::counted_iterator<std::vector<int>::iterator> ip{p.begin(), 2};
    std::counted_iterator<std::vector<int>::iterator> iq{q.begin(), 3};

    std::cout << *ip << ' ' << *iq << '\n';
    iter_swap(ip, iq); // ADL
    std::cout << *ip << ' ' << *iq << '\n';

    std::list x{0, 1, 3};
    std::counted_iterator<std::list<int>::iterator> ix{x.begin(), 2};
//  iter_swap(ip, ix); // error: not indirectly swappable
}
```

Output:

```text
1 5
5 1
```

### See also

- **swap** — swaps the values of two objects (function template)
- **swap_ranges** — swaps two ranges of elements (function template)
- **iter_swap** — swaps the elements pointed to by two iterators (function
  template)
- **iter_swap (C++20)** — swaps the values referenced by two dereferenceable
  objects (customization point object)
- **iter_move (C++20)** — casts the result of dereferencing the underlying
  iterator to its associated rvalue reference type (function)

---
*Source: https://en.cppreference.com/w/cpp/iterator/counted_iterator/iter_swap*
