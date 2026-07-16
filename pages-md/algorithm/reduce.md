# std::reduce

`std::accumulate` with the order left unspecified: elements may be
grouped and combined in any order, so `binary_op` must be associative
and commutative. That relaxation is what lets `reduce` accept an
execution policy and run in parallel (C++17) — `accumulate` never can,
because it promises strict left-to-right order.

```cpp skip
std::reduce(first, last);                        // sum via operator+, init = T{}
std::reduce(first, last, init);                   // sum via operator+
std::reduce(first, last, init, binary_op);        // custom fold
std::reduce(policy, first, last, ...);            // parallel forms (since C++17)
```

`constexpr` since C++20 (non-policy overloads).

### What you provide

- **first, last** — the range to reduce.
- **init** — the starting value; also fixes the return type `T`. If
  omitted, `T{}` (value-initialized) is used.
- **binary_op** — a callable combining two values into one (default
  `+`). It is applied in unspecified order and grouping — it must be
  associative and commutative, or the result is non-deterministic.
- **policy** — an execution policy (e.g. `std::execution::par`) to run
  in parallel; requires `<execution>`.

### Guarantees and costs

- O(last - first) applications of `binary_op`.
- Behavior is non-deterministic (not merely "in some fixed but
  unspecified order") if `binary_op` is not associative or
  commutative — different runs can give different answers.
- Undefined behavior if `binary_op` modifies any element or
  invalidates any iterator in the range, including the end iterator.
- Parallel overloads: if the invoked function throws and the policy is
  one of the standard policies, `std::terminate` is called; allocation
  failure throws `std::bad_alloc`.
- On an empty range, `init` is returned unmodified.

### Gotchas

- Passing an `int` `init` (e.g. `0`) on a range of `double` truncates
  every intermediate result back to `int` — pass `0.0` when you want
  floating-point accumulation.
- Using `reduce` with a non-associative operation (e.g.
  floating-point subtraction, or squaring where `a + b*b != b + a*a`)
  silently produces different, wrong-looking totals depending on how
  the implementation happened to group the work — especially visible
  under a parallel policy. If the operation isn't associative and
  commutative, use `std::accumulate` or `std::transform_reduce`
  instead.

### Example

```cpp
#include <iostream>
#include <numeric>
#include <vector>

int main()
{
    std::vector<int> v{1, 2, 3, 4, 5};

    int sum1 = std::accumulate(v.begin(), v.end(), 0);
    int sum2 = std::reduce(v.begin(), v.end(), 0);

    std::cout << "accumulate: " << sum1 << '\n';
    std::cout << "reduce: " << sum2 << '\n';
}
```

```text
accumulate: 15
reduce: 15
```

### Reference

```cpp skip
template< class InputIt >
typename std::iterator_traits<InputIt>::value_type
    reduce( InputIt first, InputIt last );  // (since C++17) (until C++20)
template< class InputIt >
constexpr typename std::iterator_traits<InputIt>::value_type
    reduce( InputIt first, InputIt last );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt >
typename std::iterator_traits<ForwardIt>::value_type
    reduce( ExecutionPolicy&& policy,
            ForwardIt first, ForwardIt last );  // (since C++17)
template< class InputIt, class T >
T reduce( InputIt first, InputIt last, T init );  // (since C++17) (until C++20)
template< class InputIt, class T >
constexpr T reduce( InputIt first, InputIt last, T init );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class T >
T reduce( ExecutionPolicy&& policy,
          ForwardIt first, ForwardIt last, T init );  // (since C++17)
template< class InputIt, class T, class BinaryOp >
T reduce( InputIt first, InputIt last, T init, BinaryOp binary_op );  // (since C++17) (until C++20)
template< class InputIt, class T, class BinaryOp >
constexpr T reduce( InputIt first, InputIt last, T init, BinaryOp binary_op );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class T, class BinaryOp >
T reduce( ExecutionPolicy&& policy,
          ForwardIt first, ForwardIt last, T init, BinaryOp binary_op );  // (since C++17)
```

Return value: the generalized sum GSUM(op, a1, ..., aN), defined
recursively as a1 when N = 1, and as
`op(GSUM(op, b1..bK), GSUM(op, bM..bN))` for some partitioning when
N > 1 — i.e. `accumulate` with grouping left to the implementation.
`InputIt` must meet LegacyInputIterator, `ForwardIt` must meet
LegacyForwardIterator, and `T` must be MoveConstructible with
`binary_op`'s results convertible back to `T`.

### See also

- **accumulate** — sums up or folds a range of elements, in guaranteed
  order
- **transform** — applies a function to a range, storing results in a
  destination range
- **transform_reduce** — applies a transform, then reduces out of
  order (C++17)
- **ranges::fold_left** — left-folds a range of elements, order
  guaranteed (C++23)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/reduce*
