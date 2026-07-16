# std::partial_sum

Answers: "what's the running total at each position?" — writes, at
each output position `i`, the sum (or custom-fold) of all input
elements up to and including position `i`. Its C++17 parallel-era
sibling is `std::inclusive_scan`, which relaxes the ordering guarantee
in exchange for parallelism.

```cpp skip
std::partial_sum(first, last, d_first);       // running sum, uses operator+
std::partial_sum(first, last, d_first, op);   // running fold with op
```

`constexpr` since C++20.

### What you provide

- **first, last** — the input range.
- **d_first** — start of the output range; may equal `first` to sum in
  place.
- **op** — a binary function combining the running accumulator with
  the next element (default `+`).

### Guarantees and costs

- If `[first, last)` is empty, nothing is written and `d_first` is
  returned unchanged.
- Otherwise exactly `N - 1` applications of `op` (or `operator+`),
  where `N` is the input size — O(N).
- An internal accumulator, typed as the input's value type, is written
  to each output position and fed into the next application of `op`;
  since C++11 it is moved rather than copied between steps.
- Undefined behavior if `op` modifies any element in the range or
  invalidates any iterator, including the end iterators.
- Returns an iterator one past the last element written.

### Gotchas

- The accumulator's type is the *input's* value type, not the
  output's or `op`'s return type. If those types differ (e.g. summing
  `char`s into an `int` output with a `long`-returning `op`), each
  step still round-trips through the input's type — surprising
  overflow/truncation is possible. Prefer matching types, or pick `op`
  deliberately (e.g. `std::multiplies<long>{}` on a `char` array to
  force wider intermediate arithmetic).
- `d_first == first` is explicitly supported for an in-place running
  sum; other overlaps between input and output are not.

### Example

```cpp
#include <functional>
#include <iostream>
#include <numeric>
#include <vector>

int main()
{
    std::vector<int> v(5, 2); // 2 2 2 2 2

    std::partial_sum(v.begin(), v.end(), v.begin());
    for (int n : v)
        std::cout << n << ' ';
    std::cout << '\n';

    std::partial_sum(v.begin(), v.end(), v.begin(), std::multiplies<int>());
    for (int n : v)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

```text
2 4 6 8 10 
2 8 48 384 3840
```

### Reference

```cpp skip
template< class InputIt, class OutputIt >
OutputIt partial_sum( InputIt first, InputIt last, OutputIt d_first );  // (until C++20)
template< class InputIt, class OutputIt >
constexpr OutputIt partial_sum( InputIt first, InputIt last,
                                OutputIt d_first );  // (since C++20)
template< class InputIt, class OutputIt, class BinaryOperation >
OutputIt partial_sum( InputIt first, InputIt last,
                      OutputIt d_first, BinaryOperation op );  // (until C++20)
template< class InputIt, class OutputIt, class BinaryOperation >
constexpr OutputIt partial_sum( InputIt first, InputIt last,
                                OutputIt d_first, BinaryOperation op );  // (since C++20)
```

`InputIt` must meet LegacyInputIterator with a value type
constructible from `*first`; `OutputIt` must meet LegacyOutputIterator
and accept the accumulator's type. `op` must not modify the ranges
involved or invalidate their iterators (LWG 242); the type
requirements needed for the intermediate assignments to be valid were
added by LWG 539; since C++11 the accumulator is moved rather than
copied between steps (LWG 2055 / P0616R0).

### See also

- **adjacent_difference** — computes the differences between adjacent
  elements in a range
- **accumulate** — sums up or folds a range of elements into one value
- **inclusive_scan** — parallel-friendly version; includes the ith
  input element in the ith sum (C++17)
- **exclusive_scan** — parallel-friendly version; excludes the ith
  input element from the ith sum (C++17)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/partial_sum*
