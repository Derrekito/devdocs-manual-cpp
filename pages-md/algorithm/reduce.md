# std::reduce

```cpp
template< class InputIt >
typename std::iterator_traits<InputIt>::value_type
    reduce( InputIt first, InputIt last );  // (since C++17) (until C++20)
template< class InputIt >
constexpr typename std::iterator_traits<InputIt>::value_type
    reduce( InputIt first, InputIt last );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt >
typename std::iterator_traits<ForwardIt>::value_type
    reduce( ExecutionPolicy&& policy,
            ForwardIt first, ForwardIt last );  // (2) (since C++17)
template< class InputIt, class T >
T reduce( InputIt first, InputIt last, T init );  // (since C++17) (until C++20)
template< class InputIt, class T >
constexpr T reduce( InputIt first, InputIt last, T init );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class T >
T reduce( ExecutionPolicy&& policy,
          ForwardIt first, ForwardIt last, T init );  // (4) (since C++17)
template< class InputIt, class T, class BinaryOp >
T reduce( InputIt first, InputIt last, T init, BinaryOp binary_op );  // (since C++17) (until C++20)
template< class InputIt, class T, class BinaryOp >
constexpr T reduce( InputIt first, InputIt last, T init, BinaryOp binary_op );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class T, class BinaryOp >
T reduce( ExecutionPolicy&& policy,
          ForwardIt first, ForwardIt last, T init, BinaryOp binary_op );  // (6) (since C++17)
```

1) same as `reduce(first, last, typename
   std::iterator_traits<InputIt>::value_type{})`

3) same as `reduce(first, last, init, std::plus<>())`

5) Reduces the range `[``first``,``last``)`, possibly permuted and aggregated in
   unspecified manner, along with the initial value `init` over `binary_op`.

2,4,6) Same as (1,3,5), but executed according to `policy`. These overloads do
   not participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

The behavior is non-deterministic if `binary_op` is not associative or not
commutative.

The behavior is undefined if `binary_op` modifies any element or invalidates any
iterator in `[``first``,``last``)`, including the end iterator.

### Parameters

- **first, last** — the range of elements to apply the algorithm to
- **init** — the initial value of the generalized sum
- **policy** — the execution policy to use. See execution policy for details.
- **binary_op** — binary FunctionObject that will be applied in unspecified
  order to the result of dereferencing the input iterators, the results of other
  `binary_op` and `init`.

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

**-`T` must meet the requirements of MoveConstructible. and `binary_op(init, *first)`, `binary_op(*first, init)`, `binary_op(init, init)`, and `binary_op(*first, *first)` must be convertible to `T`.**

### Return value

Generalized sum of `init` and `*first`, `*(first + 1)`, ... `*(last - 1)` over
`binary_op`,

where generalized sum GSUM(op, a1, ..., aN) is defined as follows:

- if N = 1, a1
- if N > 1, op(GSUM(op, b1, ..., bK), GSUM(op, bM, ..., bN)) where

in other words, `reduce` behaves like `std::accumulate` except the elements of
the range may be grouped and rearranged in arbitrary order

### Complexity

O(last - first) applications of `binary_op`.

### Exceptions

The overloads with a template parameter named `ExecutionPolicy` report errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Notes

If the range is empty, `init` is returned, unmodified

### Example

side-by-side comparison between `std::reduce` and `std::accumulate`:

```cpp
#if PARALLEL
#include <execution>
#define SEQ std::execution::seq,
#define PAR std::execution::par,
#else
#define SEQ
#define PAR
#endif

#include <chrono>
#include <iomanip>
#include <iostream>
#include <numeric>
#include <utility>
#include <vector>

int main()
{
    std::cout.imbue(std::locale("en_US.UTF-8"));
    std::cout << std::fixed << std::setprecision(1);
    auto eval = [](auto fun)
    {
        const auto t1 = std::chrono::high_resolution_clock::now();
        const auto [name, result] = fun();
        const auto t2 = std::chrono::high_resolution_clock::now();
        const std::chrono::duration<double, std::milli> ms = t2 - t1;
        std::cout << std::setw(28) << std::left << name << "sum: "
                  << result << "\t time: " << ms.count() << " ms\n";
    };
    {
        const std::vector<double> v(100'000'007, 0.1);

        eval([&v]{ return std::pair{"std::accumulate (double)",
            std::accumulate(v.cbegin(), v.cend(), 0.0)}; } );
        eval([&v]{ return std::pair{"std::reduce (seq, double)",
            std::reduce(SEQ v.cbegin(), v.cend())}; } );
        eval([&v]{ return std::pair{"std::reduce (par, double)",
            std::reduce(PAR v.cbegin(), v.cend())}; } );
    }
    {
        const std::vector<long> v(100'000'007, 1);

        eval([&v]{ return std::pair{"std::accumulate (long)",
            std::accumulate(v.cbegin(), v.cend(), 0l)}; } );
        eval([&v]{ return std::pair{"std::reduce (seq, long)",
            std::reduce(SEQ v.cbegin(), v.cend())}; } );
        eval([&v]{ return std::pair{"std::reduce (par, long)",
            std::reduce(PAR v.cbegin(), v.cend())}; } );
    }
}
```

Possible output:

```text
// POSIX: g++ -std=c++23 ./example.cpp -ltbb -O3; ./a.out
std::accumulate (double)    sum: 10,000,000.7    time: 356.9 ms
std::reduce (seq, double)   sum: 10,000,000.7    time: 140.1 ms
std::reduce (par, double)   sum: 10,000,000.7    time: 140.1 ms
std::accumulate (long)      sum: 100,000,007     time: 46.0 ms
std::reduce (seq, long)     sum: 100,000,007     time: 67.3 ms
std::reduce (par, long)     sum: 100,000,007     time: 63.3 ms

// POSIX: g++ -std=c++23 ./example.cpp -ltbb -O3 -DPARALLEL; ./a.out
std::accumulate (double)    sum: 10,000,000.7    time: 353.4 ms
std::reduce (seq, double)   sum: 10,000,000.7    time: 140.7 ms
std::reduce (par, double)   sum: 10,000,000.7    time: 24.7 ms
std::accumulate (long)      sum: 100,000,007     time: 42.4 ms
std::reduce (seq, long)     sum: 100,000,007     time: 52.0 ms
std::reduce (par, long)     sum: 100,000,007     time: 23.1 ms
```

### See also

- **accumulate** — sums up or folds a range of elements (function template)
- **transform** — applies a function to a range of elements, storing results in
  a destination range (function template)
- **transform_reduce (C++17)** — applies an invocable, then reduces out of order
  (function template)
- **ranges::fold_left (C++23)** — left-folds a range of elements (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/reduce*
