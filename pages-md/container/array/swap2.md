# std::swap(std::array)

```cpp
template< class T, std::size_t N >
void swap( std::array<T, N>& lhs,
           std::array<T, N>& rhs );  // (since C++11) (until C++17)
template< class T, std::size_t N >
void swap( std::array<T, N>& lhs,
           std::array<T, N>& rhs )
               noexcept(/* see below */);  // (since C++17) (until C++20)
template< class T, std::size_t N >
constexpr void swap( std::array<T, N>& lhs,
                     std::array<T, N>& rhs )
                         noexcept(/* see below */);  // (since C++20)
```

Specializes the `std::swap` algorithm for `std::array`. Swaps the contents of
`lhs` and `rhs`. Calls `lhs.swap(rhs)`.

This overload participates in overload resolution only if `N == 0` or
`std::is_swappable_v<T>` is `true`.
*(since C++17)*

### Parameters

- **lhs, rhs** — containers whose contents to swap

### Return value

(none)

### Complexity

Linear in size of the container.

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
#include <array>

int main()
{
    std::array<int, 3> alice{1, 2, 3};
    std::array<int, 3> bob{7, 8, 9};

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

Output:

```text
alice: 1 2 3
bob  : 7 8 9
-- SWAP
alice: 7 8 9
bob  : 1 2 3
```

### See also

- **swap** — swaps the contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/array/swap2*
