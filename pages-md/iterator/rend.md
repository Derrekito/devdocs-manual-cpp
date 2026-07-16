# std::rend, std::crend

```cpp
template< class C >
auto rend( C& c ) -> decltype(c.rend());  // (since C++14) (until C++17)
template< class C >
constexpr auto rend( C& c ) -> decltype(c.rend());  // (since C++17)
template< class C >
auto rend( const C& c ) -> decltype(c.rend());  // (since C++14) (until C++17)
template< class C >
constexpr auto rend( const C& c ) -> decltype(c.rend());  // (since C++17)
template< class T, std::size_t N >
std::reverse_iterator<T*> rend( T (&array)[N] );  // (since C++14) (until C++17)
template< class T, std::size_t N >
constexpr std::reverse_iterator<T*> rend( T (&array)[N] );  // (since C++17)
template< class T >
std::reverse_iterator<const T*> rend( std::initializer_list<T> il );  // (since C++14) (until C++17)
template< class T >
constexpr std::reverse_iterator<const T*> rend( std::initializer_list<T> il );  // (since C++17)
template< class C >
auto crend( const C& c ) -> decltype(std::rend(c));  // (since C++14) (until C++17)
template< class C >
constexpr auto crend( const C& c ) -> decltype(std::rend(c));  // (since C++17)
```

Returns an iterator to the reverse-end of the given range.

1) Returns an iterator to the reverse-end of the possibly const-qualified
   container or view `c`.

2) Returns `std::reverse_iterator<T*>` to the reverse-end of the array `array`.

3) Returns `std::reverse_iterator<const T*>` to the reverse-end of the
   `std::initializer_list` `il`.

4) Returns an iterator to the reverse-end of the const-qualified container or
   view `c`.

### Parameters

- **c** — a container or view with a `rend` member function
- **array** — an array of arbitrary type
- **il** — an `initializer_list`

### Return value

1) `c.rend()`

2) `std::reverse_iterator<T*>(array)`

3) `std::reverse_iterator<const T*>(il.begin())`

4) `c.rend()`

### Exceptions

May throw implementation-defined exceptions.

### Overloads

Custom overloads of `rend` may be provided for classes and enumerations that do
not expose a suitable `rend()` member function, yet can be iterated.

Overloads of `rend` found by argument-dependent lookup can be used to customize
the behavior of `std::ranges::rend` and `std::ranges::crend`.
*(since C++20)*

### Notes

The overload for `std::initializer_list` is necessary because it does not have a
member function `rend`.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    int a[]{4, 6, -3, 9, 10};
    std::cout << "C-style array `a` backwards: ";
    std::copy(std::rbegin(a), std::rend(a), std::ostream_iterator<int>(std::cout, " "));

    auto il = {3, 1, 4};
    std::cout << "\nstd::initializer_list `il` backwards: ";
    std::copy(std::rbegin(il), std::rend(il), std::ostream_iterator<int>(std::cout, " "));

    std::vector<int> v{4, 6, -3, 9, 10};
    std::cout << "\nstd::vector `v` backwards: ";
    std::copy(std::rbegin(v), std::rend(v), std::ostream_iterator<int>(std::cout, " "));
    std::cout << '\n';
}
```

Output:

```text
C-style array `a` backwards: 10 9 -3 6 4
std::initializer_list `il` backwards: 4 1 3
std::vector `v` backwards: 10 9 -3 6 4
```

### See also

- **endcend (C++11)(C++14)** — returns an iterator to the end of a container or
  array (function template)
- **rbegincrbegin (C++14)** — returns a reverse iterator to the beginning of a
  container or array (function template)
- **begincbegin (C++11)(C++14)** — returns an iterator to the beginning of a
  container or array (function template)
- **ranges::rend (C++20)** — returns a reverse end iterator to a range
  (customization point object)
- **ranges::crend (C++20)** — returns a reverse end iterator to a read-only
  range (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/iterator/rend*
