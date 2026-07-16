# std::partial_sort

Sorts just enough of a range to put its smallest `middle - first` elements,
in order, at the front — cheaper than `std::sort` when you only need the
top N, and gives you *ordered* results where `std::nth_element` would only
give you a correct partition point.

```cpp skip
std::partial_sort(first, middle, last);               // smallest N, ascending
std::partial_sort(first, middle, last, comp);          // smallest N, your order
std::partial_sort(policy, first, middle, last);        // parallel        (since C++17)
std::partial_sort(policy, first, middle, last, comp);  // parallel + comp (since C++17)
```

Since C++20 the non-policy forms are `constexpr`.

### What you provide

- **first, last** — random-access iterators bounding the whole range.
- **middle** — a random-access iterator inside `[first, last]` marking how
  many elements you want sorted: `[first, middle)` ends up holding the
  sorted smallest `middle - first` elements.
- **comp** — a callable answering *does `a` belong before `b`?*, same
  strict-weak-ordering rules as `std::sort`.
- **policy** — an execution policy such as `std::execution::par` (C++17).

### Guarantees and costs

- After the call, `[first, middle)` holds the `middle - first` smallest
  elements of the original range, fully sorted. `[middle, last)` holds the
  rest, in unspecified order.
- Equal elements are not guaranteed to keep their relative order.
- Roughly `(last - first) · log(middle - first)` comparisons — cheap when
  `middle - first` is small relative to the whole range. The typical
  implementation is heap-select then heap-sort, intended for small constant
  N.
- Parallel overloads: if a comparator throws, `std::terminate` is called;
  allocation failure throws `std::bad_alloc`.

### Gotchas

- Only `[first, middle)` is meaningful afterward — don't rely on the order
  of `[middle, last)`.
- If you need the whole range sorted, use `std::sort`; if you only need one
  element's final position (e.g. a median) without the rest ordered, use
  `std::nth_element` instead — it's linear on average.

### Example

```cpp
#include <algorithm>
#include <functional>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v{5, 7, 4, 2, 8, 6, 1, 9, 0, 3};
    std::partial_sort(v.begin(), v.begin() + 3, v.end());
    std::cout << "smallest 3: " << v[0] << ' ' << v[1] << ' ' << v[2] << '\n';

    std::vector<int> w{5, 7, 4, 2, 8, 6, 1, 9, 0, 3};
    std::partial_sort(w.begin(), w.begin() + 3, w.end(), std::greater<>{});
    std::cout << "largest 3: " << w[0] << ' ' << w[1] << ' ' << w[2] << '\n';
}
```

```text
smallest 3: 0 1 2
largest 3: 9 8 7
```

### Reference

Full declarations:

```cpp skip
template< class RandomIt >
void partial_sort( RandomIt first, RandomIt middle, RandomIt last );  // (until C++20)
template< class RandomIt >
constexpr void partial_sort( RandomIt first, RandomIt middle, RandomIt last );  // (since C++20)
template< class ExecutionPolicy, class RandomIt >
void partial_sort( ExecutionPolicy&& policy,
                   RandomIt first, RandomIt middle, RandomIt last );  // (2) (since C++17)
template< class RandomIt, class Compare >
void partial_sort( RandomIt first, RandomIt middle, RandomIt last,
                   Compare comp );  // (until C++20)
template< class RandomIt, class Compare >
constexpr void partial_sort( RandomIt first, RandomIt middle, RandomIt last,
                             Compare comp );  // (since C++20)
template< class ExecutionPolicy, class RandomIt, class Compare >
void partial_sort( ExecutionPolicy&& policy,
                   RandomIt first, RandomIt middle, RandomIt last,
                   Compare comp );  // (4) (since C++17)
```

`RandomIt` must satisfy ValueSwappable and LegacyRandomAccessIterator, and
its value type must be MoveAssignable and MoveConstructible. The policy
overloads participate in overload resolution only for genuine
execution-policy types.

### See also

- **nth_element** — partitions around one element, O(N), without sorting
- **partial_sort_copy** — same idea, writes the result to a different range
- **stable_sort** — sorts the whole range, preserving equal-element order
- **sort** — sorts the whole range into ascending order
- **ranges::partial_sort** — constrained version with projections (C++20)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/partial_sort*
