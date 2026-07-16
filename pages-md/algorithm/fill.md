# std::fill

Assigns the same `value` to every element in `[first, last)`, in
place. Reach for it whenever you'd otherwise write a hand-rolled loop
that just overwrites a range with a constant — resetting a buffer,
zeroing a `std::vector`, and similar.

```cpp skip
std::fill(first, last, value);                // assign to every element
std::fill(policy, first, last, value);         // parallel (since C++17)
```

`constexpr` since C++20.

### What you provide

- **first, last** — forward iterators bounding the range to overwrite.
- **value** — the value assigned to each element; must be writable to
  `*first` (assignable to the iterator's value type).
- **policy** — an execution policy (C++17) for parallel execution.

### Guarantees and costs

- Exactly `std::distance(first, last)` assignments.
- No return value — the range is modified in place, size unchanged.
- Parallel overload: if an assignment throws, `std::terminate` is
  called; allocation failure throws `std::bad_alloc`.

### Gotchas

- `fill` overwrites existing elements; it does not grow the
  container. Filling `[v.begin(), v.end())` on an empty vector writes
  nothing — resize first, or use `std::fill_n` with a back-inserter
  when you need to add elements.
- Requires the value type to be writable to the iterator, not merely
  CopyAssignable (LWG 283) — matters mostly for exotic proxy
  iterators.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v{0, 1, 2, 3, 4, 5, 6, 7, 8, 9};

    std::fill(v.begin(), v.end(), -1);

    for (auto elem : v)
        std::cout << elem << ' ';
    std::cout << '\n';
}
```

```text
-1 -1 -1 -1 -1 -1 -1 -1 -1 -1
```

### Reference

Full declarations:

```cpp skip
template< class ForwardIt, class T >
void fill( ForwardIt first, ForwardIt last, const T& value );  // (until C++20)
template< class ForwardIt, class T >
constexpr void fill( ForwardIt first, ForwardIt last, const T& value );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class T >
void fill( ExecutionPolicy&& policy,
           ForwardIt first, ForwardIt last, const T& value );  // (2) (since C++17)
```

`ForwardIt` must meet LegacyForwardIterator, and `value` must be
writable to `first`. LWG 283 (applied retroactively to C++98): `T`
was originally required to be CopyAssignable, which isn't always
sufficient; "writable to `ForwardIt`" is the correct requirement.

### See also

- **fill_n** — assigns a value to N elements, growable with an inserter
- **copy** — copies a range of elements to a new location
- **generate** — assigns the results of successive function calls
- **transform** — applies a function, storing results in another range
- **ranges::fill** — constrained version (C++20)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/fill*
