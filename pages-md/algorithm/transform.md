# std::transform

```cpp
template< class InputIt, class OutputIt, class UnaryOperation >
OutputIt transform( InputIt first1, InputIt last1,
                    OutputIt d_first, UnaryOperation unary_op );  // (until C++20)
template< class InputIt, class OutputIt, class UnaryOperation >
constexpr OutputIt transform( InputIt first1, InputIt last1,
                              OutputIt d_first, UnaryOperation unary_op );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1,
          class ForwardIt2, class UnaryOperation >
ForwardIt2 transform( ExecutionPolicy&& policy,
                      ForwardIt1 first1, ForwardIt1 last1,
                      ForwardIt2 d_first, UnaryOperation unary_op );  // (2) (since C++17)
template< class InputIt1, class InputIt2,
          class OutputIt, class BinaryOperation >
OutputIt transform( InputIt1 first1, InputIt1 last1, InputIt2 first2,
                    OutputIt d_first, BinaryOperation binary_op );  // (until C++20)
template< class InputIt1, class InputIt2,
          class OutputIt, class BinaryOperation >
constexpr OutputIt transform( InputIt1 first1, InputIt1 last1, InputIt2 first2,
                              OutputIt d_first, BinaryOperation binary_op );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2,
          class ForwardIt3, class BinaryOperation >
ForwardIt3 transform( ExecutionPolicy&& policy,
                      ForwardIt1 first1, ForwardIt1 last1, ForwardIt2 first2,
                      ForwardIt3 d_first, BinaryOperation binary_op );  // (4) (since C++17)
```

`std::transform` applies the given function to a range and stores the result in
another range, keeping the original elements order and beginning at `d_first`.

1) The unary operation `unary_op` is applied to the range defined by
   `[``first1``,``last1``)`.

3) The binary operation `binary_op` is applied to pairs of elements from two
   ranges: one defined by `[``first1``,``last1``)` and the other beginning at
   `first2`.

2,4) Same as (1,3), but executed according to `policy`. These overloads do not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

`unary_op` and `binary_op` must not invalidate any iterators, including the end
iterators, or modify any elements of the ranges involved.

### Parameters

- **first1, last1** — the first range of elements to transform
- **first2** — the beginning of the second range of elements to transform
- **d_first** — the beginning of the destination range, may be equal to `first1`
  or `first2`
- **policy** — the execution policy to use. See execution policy for details.
- **unary_op** — unary operation function object that will be applied. The
  signature of the function should be equivalent to the following: `Ret
  fun(const Type &a);` The signature does not need to have `const &`. The type
  Type must be such that an object of type InputIt can be dereferenced and then
  implicitly converted to Type. The type `Ret` must be such that an object of
  type `OutputIt` can be dereferenced and assigned a value of type `Ret`. ​
- **binary_op** — binary operation function object that will be applied. The
  signature of the function should be equivalent to the following: `Ret
  fun(const Type1 &a, const Type2 &b);` The signature does not need to have
  `const &`. The types Type1 and Type2 must be such that objects of types
  InputIt1 and InputIt2 can be dereferenced and then implicitly converted to
  Type1 and Type2 respectively. The type `Ret` must be such that an object of
  type `OutputIt` can be dereferenced and assigned a value of type `Ret`. ​

**Type requirements**

**-`InputIt, InputIt1, InputIt2` must meet the requirements of LegacyInputIterator.**

**-`OutputIt` must meet the requirements of LegacyOutputIterator.**

**-`ForwardIt1, ForwardIt2, ForwardIt3` must meet the requirements of LegacyForwardIterator.**

### Return value

Output iterator to the element that follows the last element transformed.

### Complexity

1,2) Exactly `std::distance(first1, last1)` applications of `unary_op`.

3,4) Exactly `std::distance(first1, last1)` applications of `binary_op`.

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
template<class InputIt, class OutputIt, class UnaryOperation>
OutputIt transform(InputIt first1, InputIt last1,
                   OutputIt d_first, UnaryOperation unary_op)
{
    while (first1 != last1)
        *d_first++ = unary_op(*first1++);

    return d_first;
}
```

```cpp
template<class InputIt1, class InputIt2,
         class OutputIt, class BinaryOperation>
OutputIt transform(InputIt1 first1, InputIt1 last1,
                   InputIt2 first2, OutputIt d_first,
                   BinaryOperation binary_op)
{
    while (first1 != last1)
        *d_first++ = binary_op(*first1++, *first2++);

    return d_first;
}
```

### Notes

`std::transform` does not guarantee in-order application of `unary_op` or
`binary_op`. To apply a function to a sequence in-order or to apply a function
that modifies the elements of a sequence, use `std::for_each`.

### Example

The following code uses `transform` to convert a string in place to uppercase
using the `std::toupper` function and then transforms each `char` to its ordinal
value:

```cpp
#include <algorithm>
#include <cctype>
#include <iomanip>
#include <iostream>
#include <string>
#include <vector>

void print_ordinals(std::vector<std::size_t> const& ordinals)
{
    std::cout << "ordinals: ";
    for (std::size_t ord : ordinals)
        std::cout << std::setw(3) << ord << ' ';
    std::cout << '\n';
}

int main()
{
    std::string s{"hello"};
    std::transform(s.cbegin(), s.cend(),
                   s.begin(), // write to the same location
                   [](unsigned char c) { return std::toupper(c); });
    std::cout << "s = " << std::quoted(s) << '\n';

    // achieving the same with std::for_each (see Notes above)
    std::string g{"hello"};
    std::for_each(g.begin(), g.end(), [](char& c) // modify in-place
    {
        c = std::toupper(static_cast<unsigned char>(c));
    });
    std::cout << "g = " << std::quoted(g) << '\n';

    std::vector<std::size_t> ordinals;
    std::transform(s.cbegin(), s.cend(), std::back_inserter(ordinals),
                   [](unsigned char c) { return c; });

    print_ordinals(ordinals);

    std::transform(ordinals.cbegin(), ordinals.cend(), ordinals.cbegin(),
                   ordinals.begin(), std::plus<>{});

    print_ordinals(ordinals);
}
```

Output:

```text
s = "HELLO"
g = "HELLO"
ordinals:  72  69  76  76  79
ordinals: 144 138 152 152 158
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 242 | C++98 | `unary_op` and `binary_op` cannot have side effects | they
      cannot modify the ranges involved

### See also

- **for_each** — applies a function to a range of elements (function template)
- **ranges::transform (C++20)** — applies a function to a range of elements
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/transform*
