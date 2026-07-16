# std::end, std::cend

Free functions that return an iterator one past the last element of a
range. For anything with an `end()` member, `std::end(c)` is just
`c.end()` — but `std::end` also works on a plain C array, returning a
pointer just past its last element, which `array.end()` cannot do.
That's why it exists: one call that works uniformly on containers and
C arrays, which matters in generic (template) code — see **begin** for
the counterpart. `std::cend` (C++14) always returns a
`const_iterator`, even when `c` itself is non-const.

```cpp skip
std::end(c)      // c.end()                          // (since C++11)
std::end(array)  // pointer past array[N-1]           // (since C++11)
std::cend(c)      // std::end(c), c treated as const  // (since C++14)
```

### What you provide

- **c** — a container or view with an `end()` member.
- **array** — a C array (`T (&)[N]`) of any element type and size.

### Guarantees and costs

- `std::end(c)` returns exactly `c.end()`: `C::iterator` if `c` is
  non-const, `C::const_iterator` if `c` is const — same type and cost
  as calling the member directly.
- `std::end(array)` returns a pointer one past the last element, in
  O(1). The end of any range is that one-past-the-last position, never
  a valid element itself.
- `std::cend(c)` returns exactly `std::end(c)` with `c` treated as
  const, so it always yields `C::const_iterator` regardless of `c`'s
  own constness.
- Container overloads are `constexpr` since C++17; the array overload
  is `constexpr` (and `noexcept`) since C++14.

### Gotchas

- Custom types without an `end()` member can still work with
  `std::end` via a free `end()` overload found by argument-dependent
  lookup — this is how `std::end` supports `std::initializer_list` and
  `std::valarray`.
- In generic code, prefer `using std::end; end(arg);` over calling
  `std::end` directly, so ADL-found overloads for user types and the
  standard templates both stay in the candidate set.
- On a shallow-const view, `std::cend` can still return a mutable
  iterator — surprising, but noted upstream (see P2276, P2278).

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

```text
Found a 5 in array w!
```

### Reference

```cpp skip
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
    -> decltype(std::end(c));  // (since C++14)
```

`cend`'s `noexcept` specification is `noexcept(noexcept(std::end(c)))`.
The standard library also supplies `end`/`cend` overloads for
`std::initializer_list`, `std::valarray`, and the filesystem
directory-iterator types, so they work in range-based `for` and with
the free-function idiom above. Since C++20, ADL-found `end` overloads
also customize `std::ranges::end` and `std::ranges::cend`.

### See also

- **begin**, **cbegin** — iterator or pointer to the first element
  (C++11, cbegin C++14)
- **ranges::end** — constrained, customization-point version (C++20)
- **ranges::cend** — read-only constrained version (C++20)

---
*Source: https://en.cppreference.com/w/cpp/iterator/end*
