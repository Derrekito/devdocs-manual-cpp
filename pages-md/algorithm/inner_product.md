# std::inner_product

Answers: "what's the sum of the pairwise products of two ranges?" — or,
with custom operations, any ordered pairwise fold across two ranges
into one accumulator. It always processes elements in order (unlike
its C++17 parallel-era sibling `std::transform_reduce`, which requires
associative/commutative ops so it can reorder).

```cpp skip
std::inner_product(first1, last1, first2, init);            // sum of products
std::inner_product(first1, last1, first2, init, op1, op2);  // custom fold
```

`constexpr` since C++20.

### What you provide

- **first1, last1** — the first range.
- **first2** — start of the second range; must have at least as many
  elements as `[first1, last1)`.
- **init** — the starting value of the accumulator, and the return
  type `T`.
- **op1** — the "combine" step: takes the running accumulator and one
  `op2` result, returns the new accumulator (default `+`).
- **op2** — the "pairwise" step: takes one element from each range,
  returns a value fed into `op1` (default `*`).

### Guarantees and costs

- Exactly `last1 - first1` applications each of `op1` and `op2`, in
  strict left-to-right order — no reordering, no parallel overload.
- Undefined behavior if `op1` or `op2` modifies any element in either
  range, or invalidates any iterator (including the end iterators).
- The accumulator is move-assigned each step (since C++11), avoiding
  extra copies for expensive `T`.

### Gotchas

- Passing an `int` `init` (e.g. `0`) truncates a `double` computation:
  every intermediate result gets converted back to `int` before the
  next step. Pass `0.0` (or the type you actually want) when the
  ranges hold floating-point data.
- Because the order is guaranteed, `inner_product` is safe with
  non-associative or non-commutative operations where
  `transform_reduce`/`reduce` would not be.

### Example

```cpp
#include <functional>
#include <iostream>
#include <numeric>
#include <vector>

int main()
{
    std::vector<int> a{0, 1, 2, 3, 4};
    std::vector<int> b{5, 4, 2, 3, 1};

    int dot = std::inner_product(a.begin(), a.end(), b.begin(), 0);
    std::cout << "dot product: " << dot << '\n';

    int matches = std::inner_product(a.begin(), a.end(), b.begin(), 0,
                                      std::plus<>(), std::equal_to<>());
    std::cout << "pairwise matches: " << matches << '\n';
}
```

```text
dot product: 21
pairwise matches: 2
```

### Reference

```cpp skip
template< class InputIt1, class InputIt2, class T >
T inner_product( InputIt1 first1, InputIt1 last1, InputIt2 first2, T init );  // (until C++20)
template< class InputIt1, class InputIt2, class T >
constexpr T inner_product( InputIt1 first1, InputIt1 last1,
                           InputIt2 first2, T init );  // (since C++20)
template< class InputIt1, class InputIt2, class T,
          class BinaryOperation1, class BinaryOperation2 >
T inner_product( InputIt1 first1, InputIt1 last1, InputIt2 first2, T init,
                 BinaryOperation1 op1, BinaryOperation2 op2 );  // (until C++20)
template< class InputIt1, class InputIt2, class T,
          class BinaryOperation1, class BinaryOperation2 >
constexpr T inner_product( InputIt1 first1, InputIt1 last1,
                           InputIt2 first2, T init,
                           BinaryOperation1 op1, BinaryOperation2 op2 );  // (since C++20)
```

`InputIt1, InputIt2` must meet LegacyInputIterator; `T` must be
CopyAssignable and CopyConstructible. `op1`/`op2` must not modify the
ranges involved or invalidate their iterators (LWG 242). Since C++11,
the accumulator is moved rather than copied while accumulating (LWG
2055 / P0616R0).

### See also

- **transform_reduce** — applies a transform, then reduces out of
  order (C++17)
- **accumulate** — sums up or folds a single range of elements
- **partial_sum** — computes the running partial sums of a range

---
*Source: https://en.cppreference.com/w/cpp/algorithm/inner_product*
