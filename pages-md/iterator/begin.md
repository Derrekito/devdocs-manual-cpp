# std::begin, std::cbegin

Free functions that return an iterator to the start of a range. For
anything with a `begin()` member, `std::begin(c)` is just `c.begin()`
— but `std::begin` also works on a plain C array, returning a pointer
to its first element, which `array.begin()` cannot do. That's the
whole point of reaching for the free function instead of the member:
write one call and it works for both containers and C arrays, which
matters in generic (template) code. `std::cbegin` (C++14) always
returns a `const_iterator`, even when `c` itself is non-const.

```cpp skip
std::begin(c)      // c.begin()                          // (since C++11)
std::begin(array)  // pointer to array[0]                 // (since C++11)
std::cbegin(c)     // std::begin(c), c treated as const   // (since C++14)
```

### What you provide

- **c** — a container or view with a `begin()` member.
- **array** — a C array (`T (&)[N]`) of any element type and size.

### Guarantees and costs

- `std::begin(c)` returns exactly `c.begin()`: `C::iterator` if `c` is
  non-const, `C::const_iterator` if `c` is const — same type and cost
  as calling the member directly.
- `std::begin(array)` returns a pointer to the first element in O(1).
- `std::cbegin(c)` returns exactly `std::begin(c)` with `c` treated as
  const, so it always yields `C::const_iterator` regardless of `c`'s
  own constness.
- Container overloads are `constexpr` since C++17; the array overload
  is `constexpr` (and `noexcept`) since C++14.

### Gotchas

- Custom types without a `begin()` member can still work with
  `std::begin` via a free `begin()` overload found by argument-
  dependent lookup — this is how `std::begin` supports
  `std::initializer_list` and `std::valarray`.
- In generic code, prefer `using std::begin; begin(arg);` over calling
  `std::begin` directly, so ADL-found overloads for user types and the
  standard templates both stay in the candidate set.
- On a shallow-const view, `std::cbegin` can still return a mutable
  iterator — surprising, but noted upstream (see P2276, P2278).

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

```text
+3
-5
```

### Reference

```cpp skip
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
    -> decltype(std::begin(c));  // (since C++14)
```

`cbegin`'s `noexcept` specification is
`noexcept(noexcept(std::begin(c)))`. The standard library also
supplies `begin`/`cbegin` overloads for `std::initializer_list`,
`std::valarray`, and the filesystem directory-iterator types, so they
work in range-based `for` and with the free-function idiom above.
Since C++20, ADL-found `begin` overloads also customize
`std::ranges::begin` and `std::ranges::cbegin`.

### See also

- **end**, **cend** — iterator or pointer past the last element
  (C++11, cend C++14)
- **ranges::begin** — constrained, customization-point version (C++20)
- **ranges::cbegin** — read-only constrained version (C++20)

---
*Source: https://en.cppreference.com/w/cpp/iterator/begin*
