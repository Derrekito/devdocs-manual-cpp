# std::iter_swap(std::reverse_iterator)

```cpp
template< std::indirectly_swappable<Iter> Iter2 >
    friend constexpr void
        iter_swap( const reverse_iterator& x,
                   const std::reverse_iterator<Iter2>& y ) noexcept(/*see below*/);  // (since C++20)
```

Swaps the objects pointed to by two adjusted underlying iterators. The function
body is equivalent to:

```cpp
auto tmp_x = x.base();
auto tmp_y = y.base();
ranges::iter_swap(--tmp_x, --tmp_y);
```

This function template is not visible to ordinary unqualified or qualified
lookup, and can only be found by argument-dependent lookup when
`std::reverse_iterator<Iter>` is an associated class of the arguments.

### Parameters

- **x, y** — reverse iterators to the elements to swap

### Return value

(none)

### Complexity

Constant.

### Exceptions

`noexcept` specification:

`noexcept( std::is_nothrow_copy_constructible_v<Iter> &&
std::is_nothrow_copy_constructible_v<Iter2> &&
noexcept(ranges::iter_swap(--std::declval<Iter&>(), --std::declval<Iter2&>()))
)`

### Example

```cpp
#include <iostream>
#include <iterator>
#include <list>
#include <vector>

int main()
{
    std::vector v{1, 2, 3};
    std::list l{4, 5, 6};

    std::reverse_iterator<std::vector<int>::iterator> r1{v.rbegin()};
    std::reverse_iterator<std::list<int>::iterator> r2{l.rbegin()};

    std::cout << *r1 << ' ' << *r2 << '\n';

    iter_swap(r1, r2); // ADL

    std::cout << *r1 << ' ' << *r2 << '\n';
}
```

Output:

```text
3 6
6 3
```

### See also

- **swap** — swaps the values of two objects (function template)
- **swap_ranges** — swaps two ranges of elements (function template)
- **iter_swap** — swaps the elements pointed to by two iterators (function
  template)
- **iter_swap (C++20)** — swaps the values referenced by two dereferenceable
  objects (customization point object)
- **iter_swap (C++20)** — swaps the objects pointed to by two underlying
  iterators (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/reverse_iterator/iter_swap*
