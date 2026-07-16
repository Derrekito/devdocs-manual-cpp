# std::get(std::pair)

```cpp
template< std::size_t I, class T1, class T2 >
typename std::tuple_element<I, std::pair<T1,T2> >::type&
    get( std::pair<T1, T2>& p ) noexcept;  // (1) (since C++11) (constexpr since C++14)
template< std::size_t I, class T1, class T2 >
const typename std::tuple_element<I, std::pair<T1,T2> >::type&
    get( const std::pair<T1,T2>& p ) noexcept;  // (2) (since C++11) (constexpr since C++14)
template< std::size_t I, class T1, class T2 >
typename std::tuple_element<I, std::pair<T1,T2> >::type&&
    get( std::pair<T1,T2>&& p ) noexcept;  // (3) (since C++11) (constexpr since C++14)
template< std::size_t I, class T1, class T2 >
const typename std::tuple_element<I, std::pair<T1,T2> >::type&&
    get( const std::pair<T1,T2>&& p ) noexcept;  // (4) (since C++11) (constexpr since C++14)
template <class T, class U>
constexpr T& get(std::pair<T, U>& p) noexcept;  // (5) (since C++14)
template <class T, class U>
constexpr const T& get(const std::pair<T, U>& p) noexcept;  // (6) (since C++14)
template <class T, class U>
constexpr T&& get(std::pair<T, U>&& p) noexcept;  // (7) (since C++14)
template <class T, class U>
constexpr const T&& get(const std::pair<T, U>&& p) noexcept;  // (8) (since C++14)
template <class T, class U>
constexpr T& get(std::pair<U, T>& p) noexcept;  // (9) (since C++14)
template <class T, class U>
constexpr const T& get(const std::pair<U, T>& p) noexcept;  // (10) (since C++14)
template <class T, class U>
constexpr T&& get(std::pair<U, T>&& p) noexcept;  // (11) (since C++14)
template <class T, class U>
constexpr const T&& get(const std::pair<U, T>&& p) noexcept;  // (12) (since C++14)
```

Extracts an element from the pair using tuple-like interface.

1-4) The index-based overloads fail to compile if the index `I` is neither `​0​`
   nor `1`.

5-12) The type-based overloads fail to compile if the types `T` and `U` are the
   same.

### Parameters

- **p** — pair whose contents to extract

### Return value

1-4) Returns a reference to `p.first` if `I == 0` and a reference to `p.second`
   if `I == 1`.

5-8) Returns a reference to `p.first`.

9-12) Returns a reference to `p.second`.

### Example

```cpp
#include <iostream>
#include <utility>

int main()
{
    auto p = std::make_pair(1, 3.14);
    std::cout << '(' << std::get<0>(p) << ", " << std::get<1>(p) << ")\n";
    std::cout << '(' << std::get<int>(p) << ", " << std::get<double>(p) << ")\n";
}
```

Output:

```text
(1, 3.14)
(1, 3.14)
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2485 | C++11 (by index) C++14 (by type) | there are no overloads for const
      pair&& | the overloads are added

### See also

- **Structured binding (C++17)** — binds the specified names to sub-objects or
  tuple elements of the initializer
- **get(std::tuple) (C++11)** — tuple accesses specified element (function
  template)
- **get(std::array) (C++11)** — accesses an element of an `array` (function
  template)
- **get(std::variant) (C++17)** — reads the value of the variant given the index
  or the type (if the type is unique), throws on error (function template)
- **get(std::ranges::subrange) (C++20)** — obtains iterator or sentinel from a
  `std::ranges::subrange` (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/pair/get*
