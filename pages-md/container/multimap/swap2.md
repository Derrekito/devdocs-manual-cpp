# std::swap(std::multimap)

```cpp
template< class Key, class T, class Compare, class Alloc >
void swap( std::multimap<Key, T, Compare, Alloc>& lhs,
           std::multimap<Key, T, Compare, Alloc>& rhs );  // (until C++17)
template< class Key, class T, class Compare, class Alloc >
void swap( std::multimap<Key, T, Compare, Alloc>& lhs,
           std::multimap<Key, T, Compare, Alloc>& rhs )
               noexcept(/* see below */);  // (since C++17)
```

Specializes the `std::swap` algorithm for `std::multimap`. Swaps the contents of
`lhs` and `rhs`. Calls `lhs.swap(rhs)`.

### Parameters

- **lhs, rhs** — containers whose contents to swap

### Return value

(none)

### Complexity

Constant.

### Exceptions

`noexcept` specification:
`noexcept(noexcept(lhs.swap(rhs)))`
*(since C++17)*

### Notes

Although the overloads of `std::swap` for container adaptors are introduced in
C++11, container adaptors can already be swapped by `std::swap` in C++98. Such
calls to `std::swap` usually have linear time complexity, but better complexity
may be provided.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <map>

int main()
{
    std::multimap<int, char> alice{{1, 'a'}, {2, 'b'}, {3, 'c'}};
    std::multimap<int, char> bob{{7, 'Z'}, {8, 'Y'}, {9, 'X'}, {10, 'W'}};

    auto print = [](std::pair<const int, char>& n)
    {
        std::cout << ' ' << n.first << ':' << n.second;
    };

    // Print state before swap
    std::cout << "alice:";
    std::for_each(alice.begin(), alice.end(), print);
    std::cout << "\n" "bob  :";
    std::for_each(bob.begin(), bob.end(), print);
    std::cout << '\n';

    std::cout << "-- SWAP\n";
    std::swap(alice, bob);

    // Print state after swap
    std::cout << "alice:";
    std::for_each(alice.begin(), alice.end(), print);
    std::cout << "\n" "bob  :";
    std::for_each(bob.begin(), bob.end(), print);
    std::cout << '\n';
}
```

Output:

```text
alice: 1:a 2:b 3:c
bob  : 7:Z 8:Y 9:X 10:W
-- SWAP
alice: 7:Z 8:Y 9:X 10:W
bob  : 1:a 2:b 3:c
```

### See also

- **swap** — swaps the contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/multimap/swap2*
