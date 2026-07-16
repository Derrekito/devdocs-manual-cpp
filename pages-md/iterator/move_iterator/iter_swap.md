# iter_swap(std::move_iterator)

```cpp
template< std::indirectly_swappable<Iter> Iter2 >
    friend constexpr void
        iter_swap( const move_iterator& x, const std::move_iterator<Iter2>& y )
            noexcept(/*see below*/);  // (since C++20)
```

Swaps the objects pointed to by two underlying iterators. The function body is
equivalent to `ranges::iter_swap(x.base(), y.base());`.

This function template is not visible to ordinary unqualified or qualified
lookup, and can only be found by argument-dependent lookup when
`std::move_iterator<Iter>` is an associated class of the arguments.

### Parameters

- **x, y** — move iterators to the elements to swap

### Return value

(none)

### Complexity

Constant.

### Exceptions

`noexcept` specification:

`noexcept(noexcept(ranges::iter_swap(x.base(), y.base())))`

### Example

```cpp
#include <iostream>
#include <iterator>
#include <string>
#include <vector>

int main()
{
    std::vector<std::string> p{"AA", "EE"},
                             q{"ⱯⱯ", "ƎƎ"};

    std::move_iterator<std::vector<std::string>::iterator>
        x = std::make_move_iterator(p.begin()),
        y = std::make_move_iterator(q.begin());

    std::cout << *x << ' ' << *y << '\n';

    iter_swap(x, y); // ADL

    std::cout << *x << ' ' << *y << '\n';
}
```

Output:

```text
AA ⱯⱯ
ⱯⱯ AA
```

### See also

- **swap** — swaps the values of two objects (function template)
- **swap_ranges** — swaps two ranges of elements (function template)
- **iter_swap** — swaps the elements pointed to by two iterators (function
  template)
- **iter_swap (C++20)** — swaps the values referenced by two dereferenceable
  objects (customization point object)
- **iter_swap (C++20)** — swaps the objects pointed to by two adjusted
  underlying iterators (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/move_iterator/iter_swap*
