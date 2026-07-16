# operator+(std::reverse_iterator)

```cpp
template< class Iter >
reverse_iterator<Iter>
    operator+( typename reverse_iterator<Iter>::difference_type n,
               const reverse_iterator<Iter>& it );  // (until C++17)
template< class Iter >
constexpr reverse_iterator<Iter>
    operator+( typename reverse_iterator<Iter>::difference_type n,
               const reverse_iterator<Iter>& it );  // (since C++17)
```

Returns the iterator `it` incremented by `n`.

### Parameters

- **n** — the number of positions to increment the iterator
- **it** — the iterator adaptor to increment

### Return value

The incremented iterator, that is `reverse_iterator<Iter>(it.base() - n)`.

### Example

```cpp
#include <iostream>
#include <iterator>
#include <list>
#include <vector>

int main()
{
    {
        std::vector v{0, 1, 2, 3};
        std::reverse_iterator<std::vector<int>::iterator>
            ri1{std::reverse_iterator{v.rbegin()}};
        std::cout << *ri1 << ' '; // 3
        std::reverse_iterator<std::vector<int>::iterator> ri2{2 + ri1};
        std::cout << *ri2 << ' '; // 1
    }
    {
        std::list l{5, 6, 7, 8};
        std::reverse_iterator<std::list<int>::iterator>
            ri1{std::reverse_iterator{l.rbegin()}};
        std::cout << *ri1 << '\n'; // 8
    //  auto ri2{2 + ri1}; // error: the underlying iterator does
                           // not model the random access iterator
    }
}
```

Output:

```text
3 1 8
```

### See also

-
  **operator++operator++(int)operator+=operator+operator--operator--(int)operator-=operator-**
  — advances or decrements the iterator (public member function)
- **operator-** — computes the distance between two iterator adaptors (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/reverse_iterator/operator%2B*
