# std::min

Returns the smaller of two values, or the smallest value in an
initializer list.

```cpp skip
std::min(a, b);           // smaller of a, b, by operator<
std::min(a, b, comp);     // smaller of a, b, by comp
std::min({a, b, c, ...}); // smallest in the list        (since C++11)
std::min({a, b, c, ...}, comp);                          // (since C++11)
```

All four overloads are `constexpr` since C++14.

### What you provide

- **a, b** — two values of the same type to compare.
- **ilist** — an initializer list of values to compare.
- **comp** — a callable answering *is `a` less than `b`?*, returning
  `bool`. Must not modify its arguments. Defaults to `operator<`.

### Guarantees and costs

- Two-argument form: exactly one comparison.
- Initializer-list form: exactly `ilist.size() - 1` comparisons.
- Returns `a` (or the leftmost element) when there's a tie — never the
  second/later equivalent value.
- The two-argument overloads return a `const T&` — a reference to
  whichever input was smaller, not a copy.

### Gotchas

- `std::min` returns a **reference**. Binding it with `const auto&`
  when an argument is a temporary produces a dangling reference the
  moment the full expression ends — returning by reference across the
  call does not extend the temporary's lifetime. Bind to `auto` (a
  copy) instead when either argument might be a temporary.
- Both arguments must be the exact same type; mixed types (e.g. `int`
  and `double`) fail template deduction rather than converting.
- For the smallest element of a *range* (not two values), use
  `std::min_element`, which returns an iterator.

### Example

```cpp
#include <algorithm>
#include <iostream>

int main()
{
    std::cout << std::min(3, 7) << ' '
              << std::min({4, 1, 8, 2}) << '\n';
}
```

```text
3 1
```

### Reference

Full declarations:

```cpp skip
template< class T >
const T& min( const T& a, const T& b );  // (1) (constexpr since C++14)
template< class T, class Compare >
const T& min( const T& a, const T& b, Compare comp );  // (2) (constexpr since C++14)
template< class T >
T min( std::initializer_list<T> ilist );  // (3) (since C++11) (constexpr since C++14)
template< class T, class Compare >
T min( std::initializer_list<T> ilist, Compare comp );  // (4) (since C++11) (constexpr since C++14)
```

`T` must be LessThanComparable for overloads (1,3); CopyConstructible
for overloads (3,4). A defect report (LWG 281, applied to C++98)
removed an earlier requirement that `T` be CopyConstructible for
overloads (1,2) as well.

### See also

- **max** — returns the greater of the given values
- **minmax (C++11)** — smaller and larger of two elements, one pass
- **min_element** — smallest element of a range, as an iterator
- **clamp (C++17)** — clamps a value between a pair of bounds
- **ranges::min (C++20)** — constrained version of this function

---
*Source: https://en.cppreference.com/w/cpp/algorithm/min*
