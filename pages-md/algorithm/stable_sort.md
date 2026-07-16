# std::stable_sort

Sorts a range in place like `std::sort`, but keeps the relative order of
elements that compare equal — reach for this instead of `std::sort` whenever
"equal" elements carry other data whose order matters (e.g. sorting by one
field after already sorting by another).

```cpp skip
std::stable_sort(first, last);                  // ascending, uses operator<
std::stable_sort(first, last, comp);             // your ordering
std::stable_sort(policy, first, last);           // parallel        (since C++17)
std::stable_sort(policy, first, last, comp);     // parallel + comp (since C++17)
```

Since C++26 the non-policy forms are `constexpr`.

### What you provide

- **first, last** — random-access iterators (`vector`, `array`, `deque`,
  `std::string`, or a C array).
- **comp** — a callable answering *does `a` belong before `b`?*, taking two
  elements and returning `bool`. It must be a strict weak ordering and must
  not modify its arguments.
- **policy** — an execution policy such as `std::execution::par` (C++17) to
  sort with multiple threads.

### Guarantees and costs

- Equal elements keep their original relative order — this is the entire
  reason to reach for `stable_sort` instead of `sort`.
- Complexity is O(N·log(N)²) in the worst case; if the implementation can
  allocate a temporary buffer the size of the sequence, it drops to
  O(N·log(N)). The algorithm tries to allocate that buffer first and falls
  back to the slower path only if the allocation fails.
- Parallel overloads: if a comparator throws, `std::terminate` is called;
  allocation failure throws `std::bad_alloc`.

### Gotchas

- A comparator that isn't a strict weak ordering is undefined behavior, same
  as `std::sort`.
- If equal elements' order doesn't matter to you, plain `std::sort` is
  usually faster and never worse — don't pay the stability cost by default.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>

struct Employee
{
    int age;
    std::string name; // does not participate in comparisons
};

bool operator<(const Employee& lhs, const Employee& rhs)
{
    return lhs.age < rhs.age;
}

int main()
{
    std::vector<Employee> v{{108, "Zaphod"}, {32, "Arthur"}, {108, "Ford"}};

    std::stable_sort(v.begin(), v.end());

    for (const Employee& e : v)
        std::cout << e.age << ", " << e.name << '\n';
}
```

```text
32, Arthur
108, Zaphod
108, Ford
```

### Reference

Full declarations:

```cpp skip
template< class RandomIt >
void stable_sort( RandomIt first, RandomIt last );  // (until C++26)
template< class RandomIt >
constexpr void stable_sort( RandomIt first, RandomIt last );  // (since C++26)
template< class ExecutionPolicy, class RandomIt >
void stable_sort( ExecutionPolicy&& policy,
                  RandomIt first, RandomIt last );  // (2) (since C++17)
template< class RandomIt, class Compare >
void stable_sort( RandomIt first, RandomIt last, Compare comp );  // (until C++26)
template< class RandomIt, class Compare >
constexpr void stable_sort( RandomIt first, RandomIt last, Compare comp );  // (since C++26)
template< class ExecutionPolicy, class RandomIt, class Compare >
void stable_sort( ExecutionPolicy&& policy,
                  RandomIt first, RandomIt last, Compare comp );  // (4) (since C++17)
```

A sequence is sorted with respect to `comp` when, for every valid `it` and
non-negative `n`, `comp(*(it + n), *it)` is `false`. `RandomIt` must satisfy
ValueSwappable and LegacyRandomAccessIterator, and its value type must be
MoveAssignable and MoveConstructible. The policy overloads participate in
overload resolution only for genuine execution-policy types.

### See also

- **sort** — same idea, faster, but doesn't preserve equal-element order
- **partial_sort** — sorts only the first N elements
- **stable_partition** — splits a range into two groups, preserving order
- **ranges::stable_sort** — constrained version with projections (C++20)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/stable_sort*
