# std::end, std::cend

```cpp
template< class C >
auto end( C& c ) -> decltype(c.end());  // (1) (since C++11) (constexpr since C++17)
template< class C >
auto end( const C& c ) -> decltype(c.end());  // (2) (since C++11) (constexpr since C++17)
template< class T, std::size_t N >
T* end( T (&array)[N] );  // (since C++11) (until C++14)
template< class T, std::size_t N >
constexpr T* end( T (&array)[N] ) noexcept;  // (since C++14)
template< class C >
constexpr auto cend( const C& c ) noexcept(/* see below */)
    -> decltype(std::end(c));  // (4) (since C++14)
```

Returns an iterator to the end (i.e. the element after the last element) of the
given range.

1,2) Returns exactly `c.end()`, which is typically an iterator one past the end
   of the sequence represented by `c`. If `C` is a standard Container:

   1) Returns a `C::iterator` (when `c` is not const-qualified), otherwise

   2) Returns a `C::const_iterator`.

3) Returns a pointer to the end of the array `array`.

4) Returns exactly `std::end(c)`, with `c` always treated as const-qualified. If
   `C` is a standard Container, this always returns a `C::const_iterator`.

### Parameters

- **c** — a container or view with an `end` member function
- **array** — an array of arbitrary type

### Return value

An iterator to the end of the range. Note that the end of a range is defined as
the element following the last valid element.

### Exceptions

4) `noexcept` specification: `noexcept(noexcept(std::end(c)))`

### Overloads

Custom overloads of `end` may be provided for classes and enumerations that do
not expose a suitable `end()` member function, yet can be iterated. The
following overloads are already provided by the standard library:

- **std::end(std::initializer_list) (C++11)** — specializes `std::end` (function
  template)
- **std::end(std::valarray) (C++11)** — specializes `std::end` (function
  template)
-
  **begin(std::filesystem::directory_iterator)end(std::filesystem::directory_iterator)
  (C++17)** — range-based for loop support (function)
-
  **begin(std::filesystem::recursive_directory_iterator)end(std::filesystem::recursive_directory_iterator)**
  — range-based for loop support (function)

Similar to the use of `swap` (described in Swappable), typical use of the `end`
function in generic context is an equivalent of `using std::end; end(arg);`,
which lets both the ADL-selected overloads for user-defined types and the
standard library function templates to appear in the same overload set.

```cpp
template<typename Container, typename Function>
void for_each(Container&& cont, Function f)
{
    using std::begin;
    auto it = begin(cont);
    using std::end;
    auto end_it = end(cont);

    for (; it != end_it; ++it)
        f(*it);
}
```

Overloads of `end` found by argument-dependent lookup can be used to customize
the behavior of `std::ranges::end`, `std::ranges::cend`, and other customization
pointer objects depending on `std::ranges::end`.
*(since C++20)*

### Notes

(1,4) exactly reflect the behavior of `C::end()`. Their effects may be
surprising if the member function does not have a reasonable implementation.

`std::cend` is introduced for unification of member and non-member range
accesses. See also LWG issue 2128.

If `C` is a shallow-const view, `std::cend` may return a mutable iterator. Such
behavior is unexpected for some users. See also P2276 and P2278.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v = {3, 1, 4};
    if (std::find(std::begin(v), std::end(v), 5) != std::end(v))
        std::cout << "Found a 5 in vector v!\n";

    int w[] = {5, 10, 15};
    if (std::find(std::begin(w), std::end(w), 5) != std::end(w))
        std::cout << "Found a 5 in array w!\n";
}
```

Output:

```text
Found a 5 in array w!
```

### See also

- **begincbegin (C++11)(C++14)** — returns an iterator to the beginning of a
  container or array (function template)
- **ranges::end (C++20)** — returns a sentinel indicating the end of a range
  (customization point object)
- **ranges::cend (C++20)** — returns a sentinel indicating the end of a
  read-only range (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/iterator/end*
