# std::transform_reduce

```cpp
template< class InputIt1, class InputIt2, class T >
T transform_reduce( InputIt1 first1, InputIt1 last1,
                    InputIt2 first2,
                    T init );  // (since C++17) (until C++20)
template< class InputIt1, class InputIt2, class T >
constexpr
T transform_reduce( InputIt1 first1, InputIt1 last1,
                    InputIt2 first2,
                    T init );  // (since C++20)
template< class InputIt1, class InputIt2,
          class T,
          class BinaryReductionOp,
          class BinaryTransformOp >
T transform_reduce( InputIt1 first1, InputIt1 last1,
                    InputIt2 first2,
                    T init,
                    BinaryReductionOp reduce,
                    BinaryTransformOp transform );  // (since C++17) (until C++20)
template< class InputIt1, class InputIt2,
          class T,
          class BinaryReductionOp,
          class BinaryTransformOp >
constexpr
T transform_reduce( InputIt1 first1, InputIt1 last1,
                    InputIt2 first2,
                    T init,
                    BinaryReductionOp reduce,
                    BinaryTransformOp transform );  // (since C++20)
template< class InputIt,
          class T,
          class BinaryReductionOp,
          class UnaryTransformOp >
T transform_reduce( InputIt first, InputIt last,
                    T init,
                    BinaryReductionOp reduce,
                    UnaryTransformOp transform );  // (since C++17) (until C++20)
template< class InputIt, class T,
          class BinaryReductionOp,
          class UnaryTransformOp >
constexpr
T transform_reduce( InputIt first, InputIt last,
                    T init,
                    BinaryReductionOp reduce,
                    UnaryTransformOp transform );  // (since C++20)
template< class ExecutionPolicy,
          class ForwardIt1, class ForwardIt2, class T >
T transform_reduce( ExecutionPolicy&& policy,
                    ForwardIt1 first1, ForwardIt1 last1,
                    ForwardIt2 first2,
                    T init );  // (4) (since C++17)
template< class ExecutionPolicy,
          class ForwardIt1, class ForwardIt2, class T,
          class BinaryReductionOp,
          class BinaryTransformOp >
T transform_reduce( ExecutionPolicy&& policy,
                    ForwardIt1 first1, ForwardIt1 last1,
                    ForwardIt2 first2,
                    T init,
                    BinaryReductionOp reduce,
                    BinaryTransformOp transform );  // (5) (since C++17)
template< class ExecutionPolicy,
          class ForwardIt, class T,
          class BinaryReductionOp,
          class UnaryTransformOp >
T transform_reduce( ExecutionPolicy&& policy,
                    ForwardIt first, ForwardIt last,
                    T init,
                    BinaryReductionOp reduce,
                    UnaryTransformOp transform );  // (6) (since C++17)
```

1) Equivalent to `std::transform_reduce(first1, last1, first2, init,
   std::plus<>(), std::multiplies<>());`, effectively parallelized version of
   the default `std::inner_product`.

2) Applies `transform` to each pair of elements from the ranges
   `[``first``,``last``)` and the range starting at `first2` and reduces the
   results (possibly permuted and aggregated in unspecified manner) along with
   the initial value `init` over `reduce`.

3) Applies `transform` to each element in the range `[``first``,``last``)` and
   reduces the results (possibly permuted and aggregated in unspecified manner)
   along with the initial value `init` over `reduce`.

4-6) Same as (1-3), but executed according to `policy`. These overloads do not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

The behavior is non-deterministic if `reduce` is not associative or not
commutative.

The behavior is undefined if `reduce`, or `transform` modifies any element or
invalidates any iterator in the input ranges, including their end iterators.

### Parameters

- **first, last** — the range of elements to apply the algorithm to
- **init** — the initial value of the generalized sum
- **policy** — the execution policy to use. See execution policy for details.
- **reduce** — binary FunctionObject that will be applied in unspecified order
  to the results of `transform`, the results of other `reduce` and `init`.
- **transform** — unary or binary FunctionObject that will be applied to each
  element of the input range(s). The return type must be acceptable as input to
  `reduce`.

**Type requirements**

**-`T` must meet the requirements of MoveConstructible in order to use overloads (3,6). and the result of the expressions `reduce(init, transform(*first))`, `reduce(transform(*first), init)`, `reduce(init, init)`, and `reduce(transform(*first), transform(*first))` must be convertible to T.**

**-`T` must meet the requirements of MoveConstructible in order to use overloads (2,5). and the result of the expressions `reduce(init, transform(*first1, *first2))`, `reduce(transform(*first1, *first2), init)`, `reduce(init, init)`, and `reduce(transform(*first1, *first2), transform(*first1, *first2))` must be convertible to T.**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

### Return value

2) Generalized sum of `init` and `transform(*first, *first2)`,
   `transform(*(first + 1), *(first2 + 1))`, ..., over `reduce`.

3) Generalized sum of `init` and `transform(*first)`, `transform(*(first + 1))`,
   ... `transform(*(last - 1))` over `reduce`,

where generalized sum GSUM(op, a1, ..., aN) is defined as follows:

- if N = 1, a1
- if N > 1, op(GSUM(op, b1, ..., bK), GSUM(op, bM, ..., bN)) where

in other words, the results of transform or of reduce may be grouped and
arranged in arbitrary order.

### Complexity

1,2,4,5) O(last1 - first1) applications each of `reduce` and `transform`.

3,6) O(last - first) applications each of `transform` and `reduce`.

### Exceptions

The overloads with a template parameter named `ExecutionPolicy` report errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Notes

In the unary-binary overload (3,6), `transform` is not applied to `init`.

If `first == last` or `first1 == last1`, `init` is returned, unmodified.

### Example

`transform_reduce` can be used to parallelize `std::inner_product`. Some systems
may need additional support to get advantages of parallel execution. E.g., on
GNU/Linux, the Intel TBB be installed and `-ltbb` option be provided to
gcc/clang compiler.

```cpp
#if PARALLEL
#include <execution>
#define PAR std::execution::par,
#else
#define PAR
#endif

#include <algorithm>
#include <functional>
#include <iostream>
#include <iterator>
#include <locale>
#include <numeric>
#include <vector>

// to parallelize non-associate accumulative operation, you'd better choose
// transform_reduce instead of reduce; e.g., a + b * b != b + a * a
void print_sum_squared(long const num)
{
    std::cout.imbue(std::locale{"en_US.UTF8"});
    std::cout << "num = " << num << '\n';

    // create an immutable vector filled with pattern: 1,2,3,4, 1,2,3,4 ...
    const std::vector<long> v { [n = num * 4] {
        std::vector<long> v;
        v.reserve(n);
        std::generate_n(std::back_inserter(v), n,
            [i = 0]() mutable { return 1 + i++ % 4; });
        return v;
    }()};

    auto squared_sum = [](auto sum, auto val) { return sum + val * val; };

    auto sum1 = std::accumulate(v.cbegin(), v.cend(), 0L, squared_sum);
    std::cout << "accumulate(): " << sum1 << '\n';

    auto sum2 = std::reduce(PAR v.cbegin(), v.cend(), 0L, squared_sum);
    std::cout << "reduce(): " << sum2 << '\n';

    auto sum3 = std::transform_reduce(PAR v.cbegin(), v.cend(), 0L, std::plus{},
                                      [](auto val) { return val * val; });
    std::cout << "transform_reduce(): " << sum3 << "\n\n";
}

int main()
{
    print_sum_squared(1);
    print_sum_squared(1'000);
    print_sum_squared(1'000'000);
}
```

Possible output:

```text
num = 1
accumulate(): 30
reduce(): 30
transform_reduce(): 30

num = 1,000
accumulate(): 30,000
reduce(): -7,025,681,278,312,630,348
transform_reduce(): 30,000

num = 1,000,000
accumulate(): 30,000,000
reduce(): -5,314,886,882,370,003,032
transform_reduce(): 30,000,000

// Compile-options for parallel execution on POSIX:
// g++ -O2 -std=c++17 -Wall -Wextra -pedantic -DPARALLEL ./example.cpp -ltbb -o tr; ./tr
```

### See also

- **accumulate** — sums up or folds a range of elements (function template)
- **transform** — applies a function to a range of elements, storing results in
  a destination range (function template)
- **reduce (C++17)** — similar to `std::accumulate`, except out of order
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/transform_reduce*
