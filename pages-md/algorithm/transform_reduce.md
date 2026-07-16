# std::transform_reduce

`std::accumulate` (or `std::inner_product`) with the order left
unspecified: it applies a transform to each element (or pair of
elements) and folds the results together in unspecified grouping and
order. That relaxation — `reduce` must be associative and commutative
— is what lets it accept an execution policy and run in parallel
(C++17). The default form is effectively a parallelizable
`std::inner_product`.

```cpp skip
// two-range form: pairwise transform then reduce (default: dot product)
std::transform_reduce(first1, last1, first2, init);
std::transform_reduce(first1, last1, first2, init, reduce, transform);

// one-range form: unary transform then reduce
std::transform_reduce(first, last, init, reduce, transform);

// any form + execution policy               (since C++17)
std::transform_reduce(policy, first1, last1, first2, init);
```

`constexpr` since C++20 (non-policy overloads).

### What you provide

- **first, last** / **first1, last1, first2** — one range, or two
  ranges of equal length to combine element-wise.
- **init** — the starting value; fixes the return type `T`.
- **reduce** — combines the running accumulator with a `transform`
  result (default `+`); applied in unspecified order, so it must be
  associative and commutative.
- **transform** — unary (one-range form) or binary (two-range form)
  callable producing the value fed into `reduce`. It is not applied to
  `init` itself. Default for the two-range, no-`op` overload is `*`.
- **policy** — an execution policy (e.g. `std::execution::par`) for
  parallel execution; requires `<execution>`.

### Guarantees and costs

- O(last - first) applications each of `transform` and `reduce`
  (O(last1 - first1) for the two-range form).
- Non-deterministic result if `reduce` is not associative or
  commutative.
- Undefined behavior if `reduce` or `transform` modifies any element
  or invalidates any iterator in the input range(s), including the
  end iterators.
- Parallel overloads: if the invoked function throws and the policy is
  one of the standard policies, `std::terminate` is called; allocation
  failure throws `std::bad_alloc`.
- If the range is empty, `init` is returned unmodified.

### Gotchas

- Same int/double `init` truncation trap as `accumulate`/`reduce`:
  `std::transform_reduce(v.begin(), v.end(), 0, ...)` on `double` data
  truncates every intermediate result to `int` — pass `0.0` when the
  transform produces floating-point values.
- Where a fold is not associative and commutative (e.g. squaring and
  summing, since `a + b*b != b + a*a`), prefer `transform_reduce` over
  `reduce` with a combined lambda: separating the per-element
  `transform` from the combining `reduce` keeps the associative
  operation (`+`) as the thing being reordered, rather than reordering
  a non-associative one. Using plain `reduce` for such a computation
  risks silently wrong, run-dependent totals — especially under a
  parallel policy.

### Example

```cpp
#include <functional>
#include <iostream>
#include <numeric>
#include <vector>

int main()
{
    std::vector<long> v{1, 2, 3, 4};

    long dot = std::transform_reduce(v.begin(), v.end(), v.begin(), 0L);

    long sum_sq = std::transform_reduce(v.begin(), v.end(), 0L, std::plus<>(),
                                         [](long x) { return x * x; });

    std::cout << "dot product: " << dot << '\n';
    std::cout << "sum of squares: " << sum_sq << '\n';
}
```

```text
dot product: 30
sum of squares: 30
```

### Reference

```cpp skip
template< class InputIt1, class InputIt2, class T >
T transform_reduce( InputIt1 first1, InputIt1 last1,
                    InputIt2 first2, T init );  // (since C++17) (until C++20)
template< class InputIt1, class InputIt2, class T >
constexpr T transform_reduce( InputIt1 first1, InputIt1 last1,
                              InputIt2 first2, T init );  // (since C++20)
template< class InputIt1, class InputIt2, class T,
          class BinaryReductionOp, class BinaryTransformOp >
T transform_reduce( InputIt1 first1, InputIt1 last1, InputIt2 first2, T init,
                    BinaryReductionOp reduce,
                    BinaryTransformOp transform );  // (since C++17) (until C++20)
template< class InputIt1, class InputIt2, class T,
          class BinaryReductionOp, class BinaryTransformOp >
constexpr T transform_reduce( InputIt1 first1, InputIt1 last1, InputIt2 first2,
                              T init, BinaryReductionOp reduce,
                              BinaryTransformOp transform );  // (since C++20)
template< class InputIt, class T,
          class BinaryReductionOp, class UnaryTransformOp >
T transform_reduce( InputIt first, InputIt last, T init,
                    BinaryReductionOp reduce,
                    UnaryTransformOp transform );  // (since C++17) (until C++20)
template< class InputIt, class T,
          class BinaryReductionOp, class UnaryTransformOp >
constexpr T transform_reduce( InputIt first, InputIt last, T init,
                              BinaryReductionOp reduce,
                              UnaryTransformOp transform );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2, class T >
T transform_reduce( ExecutionPolicy&& policy,
                    ForwardIt1 first1, ForwardIt1 last1,
                    ForwardIt2 first2, T init );  // (since C++17)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2, class T,
          class BinaryReductionOp, class BinaryTransformOp >
T transform_reduce( ExecutionPolicy&& policy,
                    ForwardIt1 first1, ForwardIt1 last1, ForwardIt2 first2,
                    T init, BinaryReductionOp reduce,
                    BinaryTransformOp transform );  // (since C++17)
template< class ExecutionPolicy, class ForwardIt, class T,
          class BinaryReductionOp, class UnaryTransformOp >
T transform_reduce( ExecutionPolicy&& policy,
                    ForwardIt first, ForwardIt last, T init,
                    BinaryReductionOp reduce,
                    UnaryTransformOp transform );  // (since C++17)
```

The no-op two-range overload is equivalent to calling it with
`std::plus<>()` as `reduce` and `std::multiplies<>()` as `transform`.
Return value is the generalized sum GSUM(op, a1, ..., aN) over the
transformed elements, defined recursively (a1 when N = 1;
`op(GSUM(...), GSUM(...))` for some partitioning when N > 1) — i.e.
grouping and order are left to the implementation. `T` must be
MoveConstructible, with `reduce`'s results convertible back to `T`;
`InputIt`/`ForwardIt` must meet the corresponding Legacy iterator
requirements.

### See also

- **accumulate** — sums up or folds a range of elements, in guaranteed
  order
- **transform** — applies a function to a range, storing results in a
  destination range
- **reduce** — like `accumulate` but out of order, without a separate
  transform step (C++17)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/transform_reduce*
