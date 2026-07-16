# std::empty

```cpp
template< class C >
constexpr auto empty( const C& c ) -> decltype(c.empty());  // (since C++17) (until C++20)
template< class C >
[[nodiscard]] constexpr auto empty( const C& c ) -> decltype(c.empty());  // (since C++20)
template< class T, std::size_t N >
constexpr bool empty( const T (&array)[N] ) noexcept;  // (since C++17) (until C++20)
template< class T, std::size_t N >
[[nodiscard]] constexpr bool empty( const T (&array)[N] ) noexcept;  // (since C++20)
template< class E >
constexpr bool empty( std::initializer_list<E> il ) noexcept;  // (since C++17) (until C++20)
template< class E >
[[nodiscard]] constexpr bool empty( std::initializer_list<E> il ) noexcept;  // (since C++20)
```

Returns whether the given range is empty.

1) Returns `c.empty()`.

2) Returns `false`.

3) Returns `il.size() == 0`.

### Parameters

- **c** — a container or view with an `empty` member function
- **array** — an array of arbitrary type
- **il** — an initializer list

### Return value

`true` if the range doesn't have any element.

### Exceptions

1) May throw implementation-defined exceptions.

### Notes

The overload for `std::initializer_list` is necessary because it does not have a
member function `empty`.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_nonmember_container_access` | 201411L | (C++17) | `std::size()`,
      `std::data()`, and `std::empty()`

### Possible implementation

```cpp
template<class C>
[[nodiscard]] constexpr auto empty(const C& c) -> decltype(c.empty())
{
    return c.empty();
}
```

```cpp
template<class T, std::size_t N>
[[nodiscard]] constexpr bool empty(const T (&array)[N]) noexcept
{
    return false;
}
```

```cpp
template<class E>
[[nodiscard]] constexpr bool empty(std::initializer_list<E> il) noexcept
{
    return il.size() == 0;
}
```

### Example

```cpp
#include <iostream>
#include <vector>

template<class T>
void print(const T& container)
{
    if (std::empty(container))
        std::cout << "Empty\n";
    else
    {
        std::cout << "Elements:";
        for (const auto& element : container)
            std::cout << ' ' << element;
        std::cout << '\n';
    }
}

int main()
{
    std::vector<int> c = {1, 2, 3};
    print(c);
    c.clear();
    print(c);

    int array[] = {4, 5, 6};
    print(array);

    auto il = {7, 8, 9};
    print(il);
}
```

Output:

```text
Elements: 1 2 3
Empty
Elements: 4 5 6
Elements: 7 8 9
```

### See also

- **ranges::empty (C++20)** — checks whether a range is empty (customization
  point object)

---
*Source: https://en.cppreference.com/w/cpp/iterator/empty*
