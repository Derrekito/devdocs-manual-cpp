# std::inclusive_scan

```cpp
template< class InputIt, class OutputIt >
OutputIt inclusive_scan( InputIt first,
                         InputIt last, OutputIt d_first );  // (since C++17) (until C++20)
template< class InputIt, class OutputIt >
constexpr OutputIt inclusive_scan( InputIt first,
                                   InputIt last, OutputIt d_first );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2 >
ForwardtIt2 inclusive_scan( ExecutionPolicy&& policy, ForwardIt1 first,
                            ForwardIt1 last, ForwardIt2 d_first );  // (2) (since C++17)
template< class InputIt, class OutputIt, class BinaryOperation >
OutputIt inclusive_scan( InputIt first, InputIt last,
                         OutputIt d_first, BinaryOperation binary_op );  // (since C++17) (until C++20)
template< class InputIt, class OutputIt, class BinaryOperation >
constexpr OutputIt inclusive_scan( InputIt first, InputIt last,
                                   OutputIt d_first, BinaryOperation binary_op );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2,
          class BinaryOperation >
ForwardIt2 inclusive_scan( ExecutionPolicy&& policy,
                           ForwardIt1 first, ForwardIt1 last,
                           ForwardIt2 d_first, BinaryOperation binary_op );  // (4) (since C++17)
template< class InputIt, class OutputIt, class BinaryOperation, class T >
OutputIt inclusive_scan( InputIt first, InputIt last, OutputIt d_first,
                         BinaryOperation binary_op, T init );  // (since C++17) (until C++20)
template< class InputIt, class OutputIt, class BinaryOperation, class T >
constexpr OutputIt inclusive_scan( InputIt first, InputIt last, OutputIt d_first,
                                   BinaryOperation binary_op, T init );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2,
          class BinaryOperation, class T >
ForwardIt2 inclusive_scan( ExecutionPolicy&& policy,
                           ForwardIt1 first, ForwardIt1 last, ForwardIt2 d_first,
                           BinaryOperation binary_op, T init );  // (6) (since C++17)
```

Computes an inclusive prefix sum operation using `binary_op` (or `std::plus<>()`
for overloads (1,2)) for the range `[``first``,``last``)`, using `init` as the
initial value (if provided), and writes the results to the range beginning at
`d_first`. "inclusive" means that the i-th input element is included in the i-th
sum.

Formally, assigns through each iterator `i` in `[``d_first``,``d_first + (last -
first)``)` the value of:

- for overloads (1-4), the generalized noncommutative sum of `*j...` for every
  `j` in `[``first``,``first + (i - d_first + 1)``)` over `binary_op`
- for overloads (5,6), the generalized noncommutative sum of `init, *j...` for
  every `j` in `[``first``,``first + (i - d_first + 1)``)` over `binary_op`

where generalized noncommutative sum GNSUM(op, a1, ..., aN) is defined as
follows:

- if N = 1, a1
- if N > 1, op(GNSUM(op, a1, ..., aK), GNSUM(op, aM, ..., aN)) for any K where 1
  < K + 1 = M ≤ N

In other words, the summation operations may be performed in arbitrary order,
and the behavior is nondeterministic if `binary_op` is not associative.
Overloads

(2,4,6) are executed according to `policy`. These overloads do not participate
in overload resolution unless

`std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`.
*(until C++20)*

`std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is `true`.
*(since C++20)*

`binary_op` shall not invalidate iterators (including the end iterators) or
subranges, nor modify elements in the ranges `[``first``,``last``)` or
`[``d_first``,``d_first + (last - first)``)`. Otherwise, the behavior is
undefined.

### Parameters

- **first, last** — the range of elements to sum
- **d_first** — the beginning of the destination range; may be equal to `first`
- **policy** — the execution policy to use. See execution policy for details.
- **init** — the initial value (optional)
- **binary_op** — binary FunctionObject that will be applied in to the result of
  dereferencing the input iterators, the results of other `binary_op`, and
  `init` (if provided)

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-`OutputIt` must meet the requirements of LegacyOutputIterator.**

**-`ForwardIt1` must meet the requirements of LegacyForwardIterator.**

**-`ForwardIt2` must meet the requirements of LegacyForwardIterator.**

**-If `init` is not provided, `decltype(first)`'s value_type must be MoveConstructible and `binary_op(*first, *first)` must be convertible to `decltype(first)`'s value type.**

**-`T (if init is provided)` must meet the requirements of MoveConstructible. All of `binary_op(init, *first)`, `binary_op(init, init)`, and `binary_op(*first, *first)` must be convertible to T.**

### Return value

Iterator to the element past the last element written.

### Complexity

O(last - first) applications of the binary operation.

### Exceptions

The overloads with a template parameter named `ExecutionPolicy` report errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Example

```cpp
#include <functional>
#include <iostream>
#include <iterator>
#include <numeric>
#include <vector>

int main()
{
    std::vector data {3, 1, 4, 1, 5, 9, 2, 6};

    std::cout << "Exclusive sum: ";
    std::exclusive_scan(data.begin(), data.end(),
                        std::ostream_iterator<int>(std::cout, " "),
                        0);
    std::cout << "\nInclusive sum: ";
    std::inclusive_scan(data.begin(), data.end(),
                        std::ostream_iterator<int>(std::cout, " "));

    std::cout << "\n\nExclusive product: ";
    std::exclusive_scan(data.begin(), data.end(),
                        std::ostream_iterator<int>(std::cout, " "),
                        1, std::multiplies<>{});
    std::cout << "\nInclusive product: ";
    std::inclusive_scan(data.begin(), data.end(),
                        std::ostream_iterator<int>(std::cout, " "),
                        std::multiplies<>{});
}
```

Output:

```text
Exclusive sum: 0 3 4 8 9 14 23 25
Inclusive sum: 3 4 8 9 14 23 25 31

Exclusive product: 1 3 3 12 12 60 540 1080
Inclusive product: 3 3 12 12 60 540 1080 6480
```

### See also

- **adjacent_difference** — computes the differences between adjacent elements
  in a range (function template)
- **accumulate** — sums up or folds a range of elements (function template)
- **partial_sum** — computes the partial sum of a range of elements (function
  template)
- **transform_inclusive_scan (C++17)** — applies an invocable, then calculates
  inclusive scan (function template)
- **exclusive_scan (C++17)** — similar to `std::partial_sum`, excludes the ith
  input element from the ith sum (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/inclusive_scan*
