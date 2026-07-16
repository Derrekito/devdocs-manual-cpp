# std::begin, std::cbegin

```cpp
template< class C >
auto begin( C& c ) -> decltype(c.begin());  // (since C++11) (until C++17)
template< class C >
constexpr auto begin( C& c ) -> decltype(c.begin());  // (since C++17)
template< class C >
auto begin( const C& c ) -> decltype(c.begin());  // (since C++11) (until C++17)
template< class C >
constexpr auto begin( const C& c ) -> decltype(c.begin());  // (since C++17)
template< class T, std::size_t N >
T* begin( T (&array)[N] );  // (since C++11) (until C++14)
template< class T, std::size_t N >
constexpr T* begin( T (&array)[N] ) noexcept;  // (since C++14)
template< class C >
constexpr auto cbegin( const C& c ) noexcept(/* see below */)
    -> decltype(std::begin(c));  // (3) (since C++14)
```

Returns an iterator to the beginning of the given range.

1) Returns exactly `c.begin()`, which is typically an iterator to the beginning
   of the sequence represented by `c`. If `C` is a standard Container, this
   returns `C::iterator` when `c` is not const-qualified, and
   `C::const_iterator` otherwise.

2) Returns a pointer to the beginning of the `array`.

3) Returns exactly `std::begin(c)`, with `c` always treated as const-qualified.
   If `C` is a standard Container, this always returns `C::const_iterator`.

### Parameters

- **c** — a container or view with a `begin` member function
- **array** — an array of arbitrary type

### Return value

An iterator to the beginning of the range.

### Exceptions

3) `noexcept` specification: `noexcept(noexcept(std::begin(c)))`

### Overloads

Custom overloads of `begin` may be provided for classes and enumerations that do
not expose a suitable `begin()` member function, yet can be iterated. The
following overloads are already provided by the standard library:

- **std::begin(std::initializer_list) (C++11)** — overloads `std::begin`
  (function template)
- **std::begin(std::valarray) (C++11)** — overloads `std::begin` (function
  template)
-
  **begin(std::filesystem::directory_iterator)end(std::filesystem::directory_iterator)
  (C++17)** — range-based for loop support (function)
-
  **begin(std::filesystem::recursive_directory_iterator)end(std::filesystem::recursive_directory_iterator)**
  — range-based for loop support (function)

Similar to the use of `swap` (described in Swappable), typical use of the
`begin` function in generic context is an equivalent of `using std::begin;
begin(arg);`, which allows both the ADL-selected overloads for user-defined
types and the standard library function templates to appear in the same overload
set.

```cpp
template<typename Container, typename Function>
void for_each(Container&& cont, Function f)
{
    using std::begin;
    auto it = begin(cont);
    using std::end;
    auto end_it = end(cont);
    while (it != end_it)
    {
        f(*it);
        ++it;
    }
}
```

Overloads of `begin` found by argument-dependent lookup can be used to customize
the behavior of `std::ranges::begin`, `std::ranges::cbegin`, and other
customization pointer objects depending on `std::ranges::begin`.
*(since C++20)*

### Notes

(1,3) exactly reflect the behavior of `C::begin()`. Their effects may be
surprising if the member function does not have a reasonable implementation.

`std::cbegin` is introduced for unification of member and non-member range
accesses. See also LWG issue 2128.

If `C` is a shallow-const view, `std::cbegin` may return a mutable iterator.
Such behavior is unexpected for some users. See also P2276 and P2278.

### Example

```cpp
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v = {3, 1, 4};
    auto vi = std::begin(v);
    std::cout << std::showpos << *vi << '\n';

    int a[] = {-5, 10, 15};
    auto ai = std::begin(a);
    std::cout << *ai << '\n';
}
```

Output:

```text
+3
-5
```

### See also

- **endcend (C++11)(C++14)** — returns an iterator to the end of a container or
  array (function template)
- **ranges::begin (C++20)** — returns an iterator to the beginning of a range
  (customization point object)
- **ranges::cbegin (C++20)** — returns an iterator to the beginning of a
  read-only range (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/iterator/begin*
