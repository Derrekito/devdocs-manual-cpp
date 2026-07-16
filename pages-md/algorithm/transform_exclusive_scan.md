# std::transform_exclusive_scan

```cpp
template< class InputIt, class OutputIt, class T,
          class BinaryOperation, class UnaryOperation >
OutputIt transform_exclusive_scan( InputIt first, InputIt last, OutputIt d_first, T init,
                                   BinaryOperation binary_op, UnaryOperation unary_op );  // (since C++17) (until C++20)
template< class InputIt, class OutputIt, class T,
          class BinaryOperation, class UnaryOperation >
constexpr OutputIt transform_exclusive_scan( InputIt first, InputIt last, OutputIt d_first,
                                             T init, BinaryOperation binary_op,
                                             UnaryOperation unary_op );  // (since C++20)
template< class ExecutionPolicy,
          class ForwardIt1, class ForwardIt2,
          class T, class BinaryOperation, class UnaryOperation >
ForwardIt2 transform_exclusive_scan( ExecutionPolicy&& policy,
                                     ForwardIt1 first, ForwardIt1 last,
                                     ForwardIt2 d_first, T init,
                                     BinaryOperation binary_op, UnaryOperation unary_op );  // (2) (since C++17)
```

Transforms each element in the range `[``first``,``last``)` with `unary_op`,
then computes an exclusive prefix sum operation using `binary_op` over the
resulting range, with `init` as the initial value, and writes the results to the
range beginning at `d_first`. "exclusive" means that the i-th input element is
not included in the i-th sum.

Formally, assigns through each iterator `i` in `[``d_first``,``d_first + (last -
first)``)` the value of the generalized noncommutative sum of `init,
unary_op(*j)...` for every `j` in `[``first``,``first + (i - d_first)``)` over
`binary_op`,

where generalized noncommutative sum GNSUM(op, a1, ..., aN) is defined as
follows:

- if N = 1, a1
- if N > 1, op(GNSUM(op, a1, ..., aK), GNSUM(op, aM, ..., aN)) for any K where 1
  < K + 1 = M ≤ N

In other words, the summation operations may be performed in arbitrary order,
and the behavior is nondeterministic if `binary_op` is not associative. Overload

(2) is executed according to `policy`. This overload does not participate in
overload resolution unless

`std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`.
*(until C++20)*

`std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is `true`.
*(since C++20)*

`unary_op` and `binary_op` shall not invalidate iterators (including the end
iterators) or subranges, nor modify elements in the ranges
`[``first``,``last``)` or `[``d_first``,``d_first + (last - first)``)`.
Otherwise, the behavior is undefined.

### Parameters

- **first, last** — the range of elements to sum
- **d_first** — the beginning of the destination range, may be equal to `first`
- **policy** — the execution policy to use. See execution policy for details.
- **init** — the initial value
- **unary_op** — unary FunctionObject that will be applied to each element of
  the input range. The return type must be acceptable as input to `binary_op`.
- **binary_op** — binary FunctionObject that will be applied in to the result of
  `unary_op`, the results of other `binary_op`, and `init`.

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-`OutputIt` must meet the requirements of LegacyOutputIterator.**

**-`ForwardIt1, ForwardIt2` must meet the requirements of LegacyForwardIterator.**

**-`T` must meet the requirements of MoveConstructible. All of `binary_op(init, unary_op(*first))`, `binary_op(init, init)`, and `binary_op(unary_op(*first), unary_op(*first))` must be convertible to `T`.**

### Return value

Iterator to the element past the last element written.

### Complexity

O(last - first) applications of each of `binary_op` and `unary_op`.

### Exceptions

The overload with a template parameter named `ExecutionPolicy` reports errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Notes

`unary_op` is not applied to `init`.

### Example

```cpp
#include <functional>
#include <iostream>
#include <iterator>
#include <numeric>
#include <vector>

int main()
{
    std::vector data{3, 1, 4, 1, 5, 9, 2, 6};

    auto times_10 = [](int x) { return x * 10; };

    std::cout << "10 times exclusive sum: ";
    std::transform_exclusive_scan(data.begin(), data.end(),
                                  std::ostream_iterator<int>(std::cout, " "),
                                  0, std::plus<int>{}, times_10);
    std::cout << "\n10 times inclusive sum: ";
    std::transform_inclusive_scan(data.begin(), data.end(),
                                  std::ostream_iterator<int>(std::cout, " "),
                                  std::plus<int>{}, times_10);
    std::cout << '\n';
}
```

Output:

```text
10 times exclusive sum: 0 30 40 80 90 140 230 250
10 times inclusive sum: 30 40 80 90 140 230 250 310
```

### See also

- **partial_sum** — computes the partial sum of a range of elements (function
  template)
- **exclusive_scan (C++17)** — similar to `std::partial_sum`, excludes the ith
  input element from the ith sum (function template)
- **transform_inclusive_scan (C++17)** — applies an invocable, then calculates
  inclusive scan (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/transform_exclusive_scan*
