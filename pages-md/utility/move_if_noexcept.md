# std::move_if_noexcept

```cpp
template< class T >
typename std::conditional<
    !std::is_nothrow_move_constructible<T>::value && std::is_copy_constructible<T>::value,
    const T&,
    T&&
>::type move_if_noexcept( T& x ) noexcept;  // (since C++11) (until C++14)
template< class T >
constexpr typename std::conditional<
    !std::is_nothrow_move_constructible<T>::value && std::is_copy_constructible<T>::value,
    const T&,
    T&&
>::type move_if_noexcept( T& x ) noexcept;  // (since C++14)
```

`std::move_if_noexcept` obtains an rvalue reference to its argument if its move
constructor does not throw exceptions or if there is no copy constructor
(move-only type), otherwise obtains an lvalue reference to its argument. It is
typically used to combine move semantics with strong exception guarantee.

### Parameters

- **x** — the object to be moved or copied

### Return value

`std::move(x)` or `x`, depending on exception guarantees.

### Complexity

Constant.

### Notes

This is used, for example, by `std::vector::resize`, which may have to allocate
new storage and then move or copy elements from old storage to new storage. If
an exception occurs during this operation, `std::vector::resize` undoes
everything it did to this point, which is only possible if
`std::move_if_noexcept` was used to decide whether to use move construction or
copy construction (unless copy constructor is not available, in which case move
constructor is used either way and the strong exception guarantee may be
waived).

### Example

```cpp
#include <iostream>
#include <utility>

struct Bad
{
    Bad() {}
    Bad(Bad&&) // may throw
    {
        std::cout << "Throwing move constructor called\n";
    }
    Bad(const Bad&) // may throw as well
    {
        std::cout << "Throwing copy constructor called\n";
    }
};

struct Good
{
    Good() {}
    Good(Good&&) noexcept // will NOT throw
    {
        std::cout << "Non-throwing move constructor called\n";
    }
    Good(const Good&) noexcept // will NOT throw
    {
        std::cout << "Non-throwing copy constructor called\n";
    }
};

int main()
{
    Good g;
    Bad b;
    [[maybe_unused]] Good g2 = std::move_if_noexcept(g);
    [[maybe_unused]] Bad b2 = std::move_if_noexcept(b);
}
```

Output:

```text
Non-throwing move constructor called
Throwing copy constructor called
```

### See also

- **forward (C++11)** — forwards a function argument (function template)
- **move (C++11)** — obtains an rvalue reference (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/move_if_noexcept*
