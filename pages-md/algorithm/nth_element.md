# std::nth_element

Puts the element that *would* be at position `nth` if the whole range were
sorted into that position, and partitions everything else around it —
cheaper than `std::partial_sort` or `std::sort` when you only need one
element's rank (a median, a kth-smallest) and don't care how the rest ends
up ordered.

```cpp skip
std::nth_element(first, nth, last);                // partition around nth, operator<
std::nth_element(first, nth, last, comp);           // partition around nth, your order
std::nth_element(policy, first, nth, last);         // parallel        (since C++17)
std::nth_element(policy, first, nth, last, comp);   // parallel + comp (since C++17)
```

The non-policy overloads (1, 3) are `constexpr` since C++20.

### What you provide

- **first, last** — random-access iterators bounding the range.
- **nth** — a random-access iterator inside `[first, last]` marking the
  position to place correctly. `nth == last` is allowed and is a no-op.
- **comp** — a callable answering *does `a` belong before `b`?*, same
  strict-weak-ordering rules as `std::sort`.
- **policy** — an execution policy such as `std::execution::par` (C++17).

### Guarantees and costs

- After the call, the element at `nth` is exactly the element that would
  occupy that position in a fully sorted range. Every element in
  `[first, nth)` compares no greater than every element in `[nth, last)`
  (formally, for `i` in `[first, nth)` and `j` in `[nth, last)`,
  `comp(*j, *i)` is `false`). Neither sub-range is otherwise sorted.
- Linear in `std::distance(first, last)` on average (overloads 1, 3). The
  parallel overloads (2, 4) do O(N) predicate applications and O(N log N)
  swaps. The typical algorithm is Introselect.
- Parallel overloads: if a comparator throws, `std::terminate` is called;
  allocation failure throws `std::bad_alloc`.

### Gotchas

- Only the element at `nth`, and the before/after split around it, are
  guaranteed — don't read order into either sub-range.
- A comparator that isn't a strict weak ordering is undefined behavior, same
  as `std::sort`.

### Example

```cpp
#include <algorithm>
#include <functional>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v{5, 10, 6, 4, 3, 2, 6, 7, 9, 3};

    auto mid = v.begin() + v.size() / 2;
    std::nth_element(v.begin(), mid, v.end());
    std::cout << "median: " << *mid << '\n';

    std::nth_element(v.begin(), v.begin() + 1, v.end(), std::greater<>{});
    std::cout << "2nd largest: " << v[1] << '\n';
    std::cout << "largest: " << v[0] << '\n';
}
```

```text
median: 6
2nd largest: 9
largest: 10
```

### Reference

Full declarations:

```cpp skip
template< class RandomIt >
void nth_element( RandomIt first, RandomIt nth, RandomIt last );  // (1) (constexpr since C++20)
template< class ExecutionPolicy, class RandomIt >
void nth_element( ExecutionPolicy&& policy,
                  RandomIt first, RandomIt nth, RandomIt last );  // (2) (since C++17)
template< class RandomIt, class Compare >
void nth_element( RandomIt first, RandomIt nth, RandomIt last,
                  Compare comp );  // (3) (constexpr since C++20)
template< class ExecutionPolicy, class RandomIt, class Compare >
void nth_element( ExecutionPolicy&& policy,
                  RandomIt first, RandomIt nth, RandomIt last,
                  Compare comp );  // (4) (since C++17)
```

`RandomIt` must satisfy ValueSwappable and LegacyRandomAccessIterator, and
its value type must be MoveAssignable and MoveConstructible. The policy
overloads participate in overload resolution only for genuine
execution-policy types.

### See also

- **max_element** — the largest element in a range, without partitioning
- **min_element** — the smallest element in a range, without partitioning
- **partial_sort_copy** — copies and partially sorts into another range
- **stable_sort** — sorts the whole range, preserving equal-element order
- **sort** — sorts the whole range into ascending order
- **ranges::nth_element** — constrained version with projections (C++20)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/nth_element*
