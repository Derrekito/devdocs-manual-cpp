# std::data

```cpp
template< class C >
constexpr auto data( C& c ) -> decltype(c.data());  // (1) (since C++17)
template< class C >
constexpr auto data( const C& c ) -> decltype(c.data());  // (2) (since C++17)
template< class T, std::size_t N >
constexpr T* data( T (&array)[N] ) noexcept;  // (3) (since C++17)
template< class E >
constexpr const E* data( std::initializer_list<E> il ) noexcept;  // (4) (since C++17)
```

Returns a pointer to the block of memory containing the elements of the range.

1,2) Returns `c.data()`.

3) Returns `array`.

4) Returns `il.begin()`.

### Parameters

- **c** — a container or view with a `data()` member function
- **array** — an array of arbitrary type
- **il** — an initializer list

### Return value

A pointer to the block of memory containing the elements of the range.

### Exceptions

1) May throw implementation-defined exceptions.

### Notes

The overload for `std::initializer_list` is necessary because it does not have a
member function `data`.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_nonmember_container_access` | 201411L | (C++17) | `std::size()`,
      `std::data()`, and `std::empty()`

### Possible implementation

```cpp
template<class C>
constexpr auto data(C& c) -> decltype(c.data())
{
    return c.data();
}
```

```cpp
template<class C>
constexpr auto data(const C& c) -> decltype(c.data())
{
    return c.data();
}
```

```cpp
template<class T, std::size_t N>
constexpr T* data(T (&array)[N]) noexcept
{
    return array;
}
```

```cpp
template<class E>
constexpr const E* data(std::initializer_list<E> il) noexcept
{
    return il.begin();
}
```

### Example

```cpp
#include <cstring>
#include <iostream>
#include <string>

int main()
{
    std::string s{"Hello world!\n"};

    char a[20]; // storage for a C-style string
    std::strcpy(a, std::data(s));
//  [s.data(), s.data() + s.size()] is guaranteed to be an NTBS since C++11

    std::cout << a;
}
```

Output:

```text
Hello world!
```

### See also

- **ranges::data (C++20)** — obtains a pointer to the beginning of a contiguous
  range (customization point object)
- **ranges::cdata (C++20)** — obtains a pointer to the beginning of a read-only
  contiguous range (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/iterator/data*
