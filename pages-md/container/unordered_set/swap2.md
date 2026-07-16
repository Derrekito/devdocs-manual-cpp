# std::swap(std::unordered_set)

```cpp
template< class Key, class Hash, class KeyEqual, class Alloc >
void swap( std::unordered_set<Key, Hash, KeyEqual, Alloc>& lhs,
           std::unordered_set<Key, Hash, KeyEqual, Alloc>& rhs );  // (since C++11) (until C++17)
template< class Key, class Hash, class KeyEqual, class Alloc >
void swap( std::unordered_set<Key, Hash, KeyEqual, Alloc>& lhs,
           std::unordered_set<Key, Hash, KeyEqual, Alloc>& rhs )
               noexcept(/* see below */);  // (since C++17)
```

Specializes the `std::swap` algorithm for `std::unordered_set`. Swaps the
contents of `lhs` and `rhs`. Calls `lhs.swap(rhs)`.

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
#include <unordered_set>

int main()
{
    std::unordered_set<int> alice{1, 2, 3};
    std::unordered_set<int> bob{7, 8, 9, 10};

    auto print = [](const int& n) { std::cout << ' ' << n; };

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

Possible output:

```text
alice: 1 2 3
bob  : 7 8 9 10
-- SWAP
alice: 7 8 9 10
bob  : 1 2 3
```

### See also

- **swap** — swaps the contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_set/swap2*
