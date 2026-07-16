# std::swap(std::stack)

```cpp
template< class T, class Container >
void swap( std::stack<T, Container>& lhs,
           std::stack<T, Container>& rhs );  // (since C++11) (until C++17)
template< class T, class Container >
void swap( std::stack<T, Container>& lhs,
           std::stack<T, Container>& rhs )
               noexcept(/* see below */);  // (since C++17)
```

Specializes the `std::swap` algorithm for `std::stack`. Swaps the contents of
`lhs` and `rhs`. Calls `lhs.swap(rhs)`.

This overload participates in overload resolution only if
`std::is_swappable_v<Container>` is `true`.
*(since C++17)*

### Parameters

- **lhs, rhs** — containers whose contents to swap

### Return value

(none)

### Complexity

Same as swapping the underlying container.

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
#include <stack>

int main()
{
    std::stack<int> alice;
    std::stack<int> bob;

    auto print = [](const auto& title, const auto& cont)
    {
        std::cout << title << " size=" << cont.size();
        std::cout << " top=" << cont.top() << '\n';
    };

    for (int i = 1; i < 4; ++i)
        alice.push(i);
    for (int i = 7; i < 11; ++i)
        bob.push(i);

    // Print state before swap
    print("alice:", alice);
    print("bob  :", bob);

    std::cout << "-- SWAP\n";
    std::swap(alice, bob);

    // Print state after swap
    print("alice:", alice);
    print("bob  :", bob);
}
```

Output:

```text
alice: size=3 top=3
bob  : size=4 top=10
-- SWAP
alice: size=4 top=10
bob  : size=3 top=3
```

### See also

- **swap (C++11)** — swaps the contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/stack/swap2*
