# std::iota

Fills a range with sequentially increasing values: writes `value` to
the first element, then repeatedly applies `++value` and writes that
to each following element. Reach for it to prime a `vector<int>` with
`0, 1, 2, ...` or to build an index array.

```cpp skip
std::iota(first, last, value);  // (since C++11)
```

`constexpr` since C++20.

### What you provide

- **first, last** — a forward range to fill.
- **value** — the starting value, copied into `*first`; the expression
  `++value` must be well-formed (works for integers, iterators, any
  incrementable type).

### Guarantees and costs

- Exactly `last - first` increments and assignments — O(N).
- Equivalent to: `*first = value; *(first+1) = ++value; ...` — each
  element gets the *post-increment* result, in order.

### Gotchas

- `value`'s type is fixed by the argument you pass; `std::iota(v, 0)`
  on a `vector<double>` still increments an `int`, then converts on
  assignment — pass the type you actually want (`0.0`) if that
  matters.
- Only fills; it does not resize. The destination range must already
  have `last - first` writable elements (or use a back-inserter-style
  output iterator only if you know it accepts sequential writes the
  way `iota` performs them — plain `iota` expects an existing range).

### Example

```cpp
#include <iostream>
#include <numeric>
#include <vector>

int main()
{
    std::vector<int> v(5);
    std::iota(v.begin(), v.end(), 10);

    for (int n : v)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

```text
10 11 12 13 14
```

### Reference

```cpp skip
template< class ForwardIt, class T >
void iota( ForwardIt first, ForwardIt last, T value );  // (until C++20)
template< class ForwardIt, class T >
constexpr void iota( ForwardIt first, ForwardIt last, T value );  // (since C++20)
```

Named after the APL integer function ⍳. It was one of the STL
components missing from C++98, added in C++11.

### See also

- **fill** — copy-assigns a fixed value to every element in a range
- **ranges::fill** — range-based version of `fill` (C++20)
- **generate** — assigns the results of successive function calls to
  every element in a range
- **ranges::generate** — range-based version of `generate` (C++20)
- **ranges::iota** — constrained version; also usable with `views::iota`
  (C++23)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/iota*
