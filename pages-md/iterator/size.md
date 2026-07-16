# std::size, std::ssize

```cpp
template< class C >
constexpr auto size( const C& c ) -> decltype(c.size());  // (1) (since C++17)
template< class C >
constexpr auto ssize( const C& c )
    -> std::common_type_t<std::ptrdiff_t,
                          std::make_signed_t<decltype(c.size())>>;  // (2) (since C++20)
template< class T, std::size_t N >
constexpr std::size_t size( const T (&array)[N] ) noexcept;  // (3) (since C++17)
template< class T, std::ptrdiff_t N >
constexpr std::ptrdiff_t ssize( const T (&array)[N] ) noexcept;  // (4) (since C++20)
```

Returns the size of the given range.

1,2) Returns `c.size()`, converted to the return type if necessary.

3,4) Returns `N`.

### Parameters

- **c** — a container or view with a `size` member function
- **array** — an array of arbitrary type

### Return value

The size of `c` or `array`.

### Exceptions

1,2) May throw implementation-defined exceptions.

### Overloads

Custom overloads of `size` may be provided for classes and enumerations that do
not expose a suitable `size()` member function, yet can be detected.

Overloads of `size` found by argument-dependent lookup can be used to customize
the behavior of `std::ranges::size`, `std::ranges::ssize`, and
`std::ranges::empty`.
*(since C++20)*

### Possible implementation

```cpp
template<class C>
constexpr auto size(const C& c) -> decltype(c.size())
{
    return c.size();
}
```

```cpp
template<class C>
constexpr auto ssize(const C& c)
    -> std::common_type_t<std::ptrdiff_t,
                          std::make_signed_t<decltype(c.size())>>
{
    using R = std::common_type_t<std::ptrdiff_t,
                                 std::make_signed_t<decltype(c.size())>>;
    return static_cast<R>(c.size());
}
```

```cpp
template<class T, std::size_t N>
constexpr std::size_t size(const T (&array)[N]) noexcept
{
    return N;
}
```

```cpp
template<class T, std::ptrdiff_t N>
constexpr std::ptrdiff_t ssize(const T (&array)[N]) noexcept
{
    return N;
}
```

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_nonmember_container_access` | 201411L | (C++17) | `std::size()`,
      `std::data` and `std::empty`
  `__cpp_lib_ssize` | 201902L | (C++20) | `std::ssize()` (2,4) and unsigned
      `std::span::size()`

### Example

```cpp
#include <cassert>
#include <iostream>
#include <vector>

int main()
{
    // Works with containers
    std::vector<int> v{3, 1, 4};
    assert(std::size(v) == 3);

    // And works with built-in arrays too
    int a[]{-5, 10, 15};
    // Returns the number of elements (not bytes) as opposed to sizeof
    assert(std::size(a) == 3);
    std::cout << "size of a[]: " << sizeof a << '\n'; // 12, if sizeof(int) == 4

    // Provides a safe way (compared to sizeof) of getting string buffer size
    const char str[] = "12345";
    // These are fine and give the correct result
    assert(std::size(str) == 6);
    assert(sizeof(str) == 6);

    // But use of sizeof here is a common source of bugs
    const char* str_decayed = "12345";
    // std::cout << std::size(str_decayed) << '\n'; // Usefully fails to compile
    std::cout << sizeof(str_decayed) << '\n'; // Prints the size of the pointer!

    // Since C++20 the signed size (std::ssize) is available
    auto i = std::ssize(v);
    for (--i; i != -1; --i)
        std::cout << v[i] << (i ? ' ' : '\n');
    assert(i == -1);
}
```

Possible output:

```text
size of a[]: 12
8
4 1 3
```

### See also

- **ptrdiff_t** — signed integer type returned when subtracting two pointers
  (typedef)
- **size_t** — unsigned integer type returned by the `sizeof` operator (typedef)
- **ranges::size (C++20)** — returns an integer equal to the size of a range
  (customization point object)
- **ranges::ssize (C++20)** — returns a signed integer equal to the size of a
  range (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/iterator/size*
