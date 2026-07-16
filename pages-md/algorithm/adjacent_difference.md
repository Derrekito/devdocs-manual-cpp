# std::adjacent_difference

```cpp
template< class InputIt, class OutputIt >
OutputIt adjacent_difference( InputIt first, InputIt last, OutputIt d_first );  // (until C++20)
template< class InputIt, class OutputIt >
constexpr OutputIt adjacent_difference( InputIt first, InputIt last,
                                        OutputIt d_first );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2 >
ForwardIt2 adjacent_difference( ExecutionPolicy&& policy,
                                ForwardIt1 first, ForwardIt1 last,
                                ForwardIt2 d_first );  // (2) (since C++17)
template< class InputIt, class OutputIt, class BinaryOperation >
OutputIt adjacent_difference( InputIt first, InputIt last,
                              OutputIt d_first, BinaryOperation op );  // (until C++20)
template< class InputIt, class OutputIt, class BinaryOperation >
constexpr OutputIt adjacent_difference( InputIt first, InputIt last,
                                        OutputIt d_first,
                                        BinaryOperation op );  // (since C++20)
template< class ExecutionPolicy,
          class ForwardIt1, class ForwardIt2, class BinaryOperation >
ForwardIt2 adjacent_difference( ExecutionPolicy&& policy,
                                ForwardIt1 first, ForwardIt1 last,
                                ForwardIt2 d_first, BinaryOperation op );  // (4) (since C++17)
```

If `[``first``,``last``)` is not empty, computes the differences between the
second and the first of each adjacent pair of its elements and writes the
differences to the range beginning at `d_first + 1`. An unmodified copy of
`*first` is written to `*d_first`. Overloads (1,2) use operator- to calculate
the differences, overloads (3,4) use the given binary function `op`, all
applying `std::move` to their operands on the right hand side(since C++11).

Equivalent operation for overload (1), if `[``first``,``last``)` is not empty,
uses the accumulator `acc` to store the value to be subtracted:

```cpp
std::iterator_traits<InputIt>::value_type acc = *first;
*d_first = acc;

std::iterator_traits<InputIt>::value_type val1 = *(first + 1);
*(d_first + 1) = val1 - std::move(acc);
// or *(d_first + 1) = op(val1, std::move(acc)); for overload (2)
acc = std::move(val1);

std::iterator_traits<InputIt>::value_type val2 = *(first + 2);
*(d_first + 2) = val2 - std::move(acc);
acc = std::move(val2);

std::iterator_traits<InputIt>::value_type val3 = *(first + 3);
*(d_first + 3) = val3 - std::move(acc);
acc = std::move(val3);
// ...
```

Equivalent operation for overload (3), if `[``first``,``last``)` is not empty:

```cpp
// performed first
*d_first = *first;

// performed after the initial assignment, might not be sequenced
*(d_first + 1) = *(first + 1) - *(first);
// or *(d_first + 1) = op(*(first + 1), *(first)); for overload (4)
*(d_first + 2) = *(first + 2) - *(first + 1);
*(d_first + 3) = *(first + 3) - *(first + 2);
...
```

If `op` invalidates any iterator (including any of the end iterators) or modify
any elements of the ranges involved, the behavior is undefined.

For overloads (1,3), if std::iterator_traits<InputIt>::value_type is not
CopyAssignable(until C++11)MoveAssignable(since C++11), the behavior is
undefined.

### Parameters

- **first, last** — the range of elements
- **d_first** — the beginning of the destination range
- **policy** — the execution policy to use. See execution policy for details.
- **op** — binary operation function object that will be applied. The signature
  of the function should be equivalent to the following: `Ret fun(const Type1
  &a, const Type2 &b);` The signature does not need to have `const &`. The types
  Type1 and Type2 must be such that an object of type
  iterator_traits<InputIt>::value_type can be implicitly converted to both of
  them. The type `Ret` must be such that an object of type `OutputIt` can be
  dereferenced and assigned a value of type `Ret`. ​

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator. For overloads (1,3), its value type must be constructible from `*first`.**

**-`OutputIt` must meet the requirements of LegacyOutputIterator. `acc` (defined above) and the result of `val - acc` (for (1)) or `op(val, acc)` (for (3))(until C++11)`val - std::move(acc)` (for (1)) or `op(val, std::move(acc))` (for (3))(since C++11) must be writable to `d_first`.**

**-`ForwardIt1, ForwardIt2` must meet the requirements of LegacyForwardIterator. The result of `*first` and the result of `*first - *first` (for (2)) or `op(*first, *first)` (for (4)) must be writable to `d_first`.**

### Return value

Iterator to the element past the last element written, or `d_first` if
`[``first``,``last``)` is empty.

### Complexity

Given `N` as `std::distance(first, last) - 1`:

1,2) exactly `N` applications of `operator-`

3,4) exactly `N` applications of the binary function `op`

### Exceptions

The overloads with a template parameter named `ExecutionPolicy` report errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Possible implementation

```cpp
template<class InputIt, class OutputIt>
constexpr // since C++20
OutputIt adjacent_difference(InputIt first, InputIt last, OutputIt d_first)
{
    if (first == last)
        return d_first;

    typedef typename std::iterator_traits<InputIt>::value_type value_t;
    value_t acc = *first;
    *d_first = acc;

    while (++first != last)
    {
        value_t val = *first;
        *++d_first = val - std::move(acc); // std::move since C++11
        acc = std::move(val);
    }

    return ++d_first;
}
```

```cpp
template<class InputIt, class OutputIt, class BinaryOperation>
constexpr // since C++20
OutputIt adjacent_difference(InputIt first, InputIt last,
                             OutputIt d_first, BinaryOperation op)
{
    if (first == last)
        return d_first;

    typedef typename std::iterator_traits<InputIt>::value_type value_t;
    value_t acc = *first;
    *d_first = acc;

    while (++first != last)
    {
        value_t val = *first;
        *++d_first = op(val, std::move(acc)); // std::move since C++11
        acc = std::move(val);
    }

    return ++d_first;
}
```

### Notes

`acc` was introduced because of the resolution of LWG issue 539. The reason of
using `acc` rather than directly calculating the differences is because the
semantic of the latter is confusing if the following types mismatch:

- the value type of `InputIt`
- the writable type(s) of `OutputIt`
- the types of the parameters of operator- or `op`
- the return type of operator- or `op`

`acc` serves as the intermediate object to cache values of the iterated
elements:

- its type is the value type of `InputIt`
- the value written to `d_first` (which is the return value of operator- or
  `op`) is assigned to it
- its value is passed to operator- or `op`

```cpp
char i_array[4] = {100, 100, 100, 100};
int  o_array[4];

// OK: performs conversions when needed
// 1. creates `acc` of type char (the value type)
// 2. `acc` is assigned to the first element of `o_array`
// 3. the char arguments are used for long multiplication (char -> long)
// 4. the long product is assigned to the output range (long -> int)
// 5. the next value of `i_array` is assigned to `acc`
// 6. go back to step 3 to process the remaining elements in the input range
std::adjacent_difference(i_array, i_array + 4, o_array, std::multiplies<long>{});
```

### Example

```cpp
#include <array>
#include <functional>
#include <iostream>
#include <iterator>
#include <numeric>
#include <vector>

auto print = [](auto comment, auto const& sequence)
{
    std::cout << comment;
    for (const auto& n : sequence)
        std::cout << n << ' ';
    std::cout << '\n';
};

int main()
{
    // Default implementation - the difference b/w two adjacent items
    std::vector v {4, 6, 9, 13, 18, 19, 19, 15, 10};
    print("Initially, v = ", v);
    std::adjacent_difference(v.begin(), v.end(), v.begin());
    print("Modified v = ", v);

    // Fibonacci
    std::array<int, 10> a {1};
    std::adjacent_difference(std::begin(a), std::prev(std::end(a)),
                             std::next(std::begin(a)), std::plus<>{});
    print("Fibonacci, a = ", a);
}
```

Output:

```text
Initially, v = 4 6 9 13 18 19 19 15 10
Modified v = 4 2 3 4 5 1 0 -4 -5
Fibonacci, a = 1 1 2 3 5 8 13 21 34 55
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 242 | C++98 | `op` could not have side effects | it cannot modify the
      ranges involved
  LWG 539 | C++98 | the type requirements needed for the result evaluations and
      assignments to be valid were missing | added
  LWG 2055 (P0616R0) | C++11 | `acc` was not moved while being accumulated | it
      is moved
  LWG 3058 | C++17 | for overloads (2,4), the result of each invocation of
      operator- or `op` was assigned to a temporary object, and that object is
      assigned to the output range | assign the results to the output range
      directly

### See also

- **partial_sum** — computes the partial sum of a range of elements (function
  template)
- **accumulate** — sums up or folds a range of elements (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/adjacent_difference*
