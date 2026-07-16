# std::rbegin, std::crbegin

```cpp
template< class C >
auto rbegin( C& c ) -> decltype(c.rbegin());  // (since C++14) (until C++17)
template< class C >
constexpr auto rbegin( C& c ) -> decltype(c.rbegin());  // (since C++17)
template< class C >
auto rbegin( const C& c ) -> decltype(c.rbegin());  // (since C++14) (until C++17)
template< class C >
constexpr auto rbegin( const C& c ) -> decltype(c.rbegin());  // (since C++17)
template< class T, std::size_t N >
std::reverse_iterator<T*> rbegin( T (&array)[N] );  // (since C++14) (until C++17)
template< class T, std::size_t N >
constexpr std::reverse_iterator<T*> rbegin( T (&array)[N] );  // (since C++17)
template< class T >
std::reverse_iterator<const T*> rbegin( std::initializer_list<T> il );  // (since C++14) (until C++17)
template< class T >
constexpr std::reverse_iterator<const T*> rbegin(std::initializer_list<T> il);  // (since C++17)
template< class C >
auto crbegin( const C& c ) -> decltype(std::rbegin(c));  // (since C++14) (until C++17)
template< class C >
constexpr auto crbegin( const C& c ) -> decltype(std::rbegin(c));  // (since C++17)
```

Returns an iterator to the reverse-beginning of the given range.

1) Returns an iterator to the reverse-beginning of the possibly const-qualified
   container or view `c`.

2) Returns `std::reverse_iterator<T*>` to the reverse-beginning of the array
   `array`.

3) Returns `std::reverse_iterator<const T*>` to the reverse-beginning of the
   `std::initializer_list` `il`.

4) Returns an iterator to the reverse-beginning of the const-qualified container
   or view `c`.

### Parameters

- **c** — a container or view with a `rbegin` member function
- **array** — an array of arbitrary type
- **il** — an `initializer_list`

### Return value

1) `c.rbegin()`

2) `std::reverse_iterator<T*>(array + N)`

3) `std::reverse_iterator<const T*>(il.end())`

4) `c.rbegin()`

### Exceptions

May throw implementation-defined exceptions.

### Overloads

Custom overloads of `rbegin` may be provided for classes and enumerations that
do not expose a suitable `rbegin()` member function, yet can be iterated.

Overloads of `rbegin` found by argument-dependent lookup can be used to
customize the behavior of `std::ranges::rbegin` and `std::ranges::crbegin`.
*(since C++20)*

### Notes

The overload for `std::initializer_list` is necessary because it does not have a
member function `rbegin`.

### Example

```cpp
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v = {3, 1, 4};
    auto vi = std::rbegin(v); // the type of `vi` is std::vector<int>::reverse_iterator
    std::cout << "*vi = " << *vi << '\n';

    *std::rbegin(v) = 42; // OK: after assignment v[2] == 42
//  *std::crbegin(v) = 13; // error: the location is read-only

    int a[] = {-5, 10, 15};
    auto ai = std::rbegin(a); // the type of `ai` is std::reverse_iterator<int*>
    std::cout << "*ai = " << *ai << '\n';

    auto il = {3, 1, 4};
    // the type of `it` below is std::reverse_iterator<int const*>:
    for (auto it = std::rbegin(il); it != std::rend(il); ++it)
        std::cout << *it << ' ';
    std::cout << '\n';
}
```

Output:

```text
*vi = 4
*ai = 15
4 1 3
```

### See also

- **begincbegin (C++11)(C++14)** — returns an iterator to the beginning of a
  container or array (function template)
- **endcend (C++11)(C++14)** — returns an iterator to the end of a container or
  array (function template)
- **rendcrend (C++14)** — returns a reverse end iterator for a container or
  array (function template)
- **ranges::rbegin (C++20)** — returns a reverse iterator to a range
  (customization point object)
- **ranges::crbegin (C++20)** — returns a reverse iterator to a read-only range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/iterator/rbegin*
