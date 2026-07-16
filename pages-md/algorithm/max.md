# std::max

Returns the greater of two values, or the greatest value in an
initializer list.

```cpp skip
std::max(a, b);           // greater of a, b, by operator<
std::max(a, b, comp);     // greater of a, b, by comp
std::max({a, b, c, ...}); // greatest in the list        (since C++11)
std::max({a, b, c, ...}, comp);                          // (since C++11)
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
  whichever input was larger, not a copy.

### Gotchas

- `std::max` returns a **reference**. Binding it with `const auto&`
  when an argument is a temporary produces a dangling reference the
  moment the full expression ends — returning by reference across the
  call does not extend the temporary's lifetime. Bind to `auto` (a
  copy) instead when either argument might be a temporary.
- Both arguments must be the exact same type; `std::max(0, 1.5)` fails
  template deduction (int vs double) rather than converting. Match the
  types, or force one with `std::max<double>(0, 1.5)`.
- For the largest element of a *range* (not two values), use
  `std::max_element`, which returns an iterator.

### Example

```cpp
#include <algorithm>
#include <iostream>

int main()
{
    int best = 0;
    for (int x : {3, -1, 9, 4})
        best = std::max(best, x);

    std::cout << best << ' ' << std::max({4, 1, 8, 2}) << '\n';
}
```

```text
9 8
```

### Reference

Full declarations:

```cpp skip
template< class T >
const T& max( const T& a, const T& b );  // (1) (constexpr since C++14)
template< class T, class Compare >
const T& max( const T& a, const T& b, Compare comp );  // (2) (constexpr since C++14)
template< class T >
T max( std::initializer_list<T> ilist );  // (3) (since C++11) (constexpr since C++14)
template< class T, class Compare >
T max( std::initializer_list<T> ilist, Compare comp );  // (4) (since C++11) (constexpr since C++14)
```

`T` must be LessThanComparable for overloads (1,3); CopyConstructible
for overloads (3,4). A defect report (LWG 281, applied to C++98)
removed an earlier requirement that `T` be CopyConstructible for
overloads (1,2) as well.

### See also

- **min** — returns the smaller of the given values
- **minmax (C++11)** — smaller and larger of two elements, one pass
- **max_element** — largest element of a range, as an iterator
- **clamp (C++17)** — clamps a value between a pair of bounds
- **ranges::max (C++20)** — constrained version of this function

---
*Source: https://en.cppreference.com/w/cpp/algorithm/max*
