# std::get(std::array)

```cpp
template< std::size_t I, class T, std::size_t N >
T& get( std::array<T,N>& a ) noexcept;  // (since C++11) (until C++14)
template< std::size_t I, class T, std::size_t N >
constexpr T& get( std::array<T,N>& a ) noexcept;  // (since C++14)
template< std::size_t I, class T, std::size_t N >
T&& get( std::array<T,N>&& a ) noexcept;  // (since C++11) (until C++14)
template< std::size_t I, class T, std::size_t N >
constexpr T&& get( std::array<T,N>&& a ) noexcept;  // (since C++14)
template< std::size_t I, class T, std::size_t N >
const T& get( const std::array<T,N>& a ) noexcept;  // (since C++11) (until C++14)
template< std::size_t I, class T, std::size_t N >
constexpr const T& get( const std::array<T,N>& a ) noexcept;  // (since C++14)
template< std::size_t I, class T, std::size_t N >
const T&& get( const std::array<T,N>&& a ) noexcept;  // (since C++11) (until C++14)
template< std::size_t I, class T, std::size_t N >
constexpr const T&& get( const std::array<T,N>&& a ) noexcept;  // (since C++14)
```

Extracts the `I`th element from the array using tuple-like interface.

`I` must be an integer value in range `[`​0​`,`N`)`. This is enforced at compile
time as opposed to `at()` or `operator[]`.

### Parameters

- **a** — array whose contents to extract

### Return value

A reference to the `I`th element of `a`.

### Complexity

Constant.

### Example

```cpp
#include <array>
#include <iostream>

int main()
{
    std::array<int, 3> arr;

    // set values:
    std::get<0>(arr) = 1;
    std::get<1>(arr) = 2;
    std::get<2>(arr) = 3;

    // get values:
    std::cout << '(' << std::get<0>(arr)
              << ',' << std::get<1>(arr)
              << ',' << std::get<2>(arr)
              << ")\n";
}
```

Output:

```text
(1,2,3)
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2485 | C++11 | there are no overloads for const array&& | the overloads
      are added

### See also

- **Structured binding (C++17)** — binds the specified names to sub-objects or
  tuple elements of the initializer
- **operator[]** — access specified element (public member function)
- **at** — access specified element with bounds checking (public member
  function)
- **get(std::tuple) (C++11)** — tuple accesses specified element (function
  template)
- **get(std::pair) (C++11)** — accesses an element of a `pair` (function
  template)
- **get(std::variant) (C++17)** — reads the value of the variant given the index
  or the type (if the type is unique), throws on error (function template)
- **get(std::ranges::subrange) (C++20)** — obtains iterator or sentinel from a
  `std::ranges::subrange` (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/array/get*
