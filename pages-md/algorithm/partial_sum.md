# std::partial_sum

```cpp
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

If `[``first``,``last``)` is not empty, computes the partial sums of the
elements in its subranges and writes the sums to the range beginning at
`d_first`, both applying `std::move` to their operands on the left hand
side(since C++11).

Internally, a variable `acc`, whose type is the value type of `InputIt`, is used
as accumulator for intermediate results.

1) Uses `operator+` to sum up the elements. Equivalent to:
   std::iterator_traits<InputIt>::value_type acc = *first; *d_first = acc; acc =
   std::move(acc) + *(first + 1); *(d_first + 1) = acc; acc = std::move(acc) +
   *(first + 2); *(d_first + 2) = acc; acc = std::move(acc) + *(first + 3);
   *(d_first + 3) = acc; // ...

2) Uses the given binary function `op`. Equivalent to:
   std::iterator_traits<InputIt>::value_type acc = *first; *d_first = acc; acc =
   op(std::move(acc), *(first + 1)); *(d_first + 1) = acc; acc =
   op(std::move(acc), *(first + 2)); *(d_first + 2) = acc; acc =
   op(std::move(acc), *(first + 3)); *(d_first + 3) = acc; // ...

If `op` invalidates any iterators (including the end iterators) or modifies any
   elements of the range involved, the behavior is undefined.

### Parameters

- **first, last** — the range of elements to sum
- **d_first** — the beginning of the destination range; may be equal to `first`
- **op** — binary operation function object that will be applied. The signature
  of the function should be equivalent to the following: `Ret fun(const Type1
  &a, const Type2 &b);` The signature does not need to have `const &`. The type
  Type1 must be such that an object of type
  std::iterator_traits<InputIt>::value_type can be implicitly converted to
  Type1. The type Type2 must be such that an object of type InputIt can be
  dereferenced and then implicitly converted to Type2. The type `Ret` must be
  such that an object of type `InputIt` can be dereferenced and assigned a value
  of type `Ret`. ​

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator. Its value type must be constructible from `*first`.**

**-`OutputIt` must meet the requirements of LegacyOutputIterator. `acc` (defined above) must be writable to `d_first`.**

### Return value

Iterator to the element past the last element written, or `d_first` if
`[``first``,``last``)` is empty.

### Complexity

Given `N` as `std::distance(first, last) - 1`:

1) exactly `N` applications of `operator+`

2) exactly `N` applications of the binary function `op`

### Possible implementation

```cpp
template<class InputIt, class OutputIt>
constexpr // since C++20
OutputIt partial_sum(InputIt first, InputIt last, OutputIt d_first)
{
    if (first == last)
        return d_first;

    typename std::iterator_traits<InputIt>::value_type sum = *first;
    *d_first = sum;

    while (++first != last)
    {
        sum = std::move(sum) + *first; // std::move since C++11
        *++d_first = sum;
    }

    return ++d_first;

    // or, since C++14:
    // return std::partial_sum(first, last, d_first, std::plus<>());
}
```

```cpp
template<class InputIt, class OutputIt, class BinaryOperation>
constexpr // since C++20
OutputIt partial_sum(InputIt first, InputIt last,
                     OutputIt d_first, BinaryOperation op)
{
    if (first == last)
        return d_first;

    typename std::iterator_traits<InputIt>::value_type acc = *first;
    *d_first = acc;

    while (++first != last)
    {
        acc = op(std::move(acc), *first); // std::move since C++11
        *++d_first = acc;
    }

    return ++d_first;
}
```

### Notes

`acc` was introduced because of the resolution of LWG issue 539. The reason of
using `acc` rather than directly summing up the results (i.e. `*(d_first + 2) =
(*first + *(first + 1)) + *(first + 2);`) is because the semantic of the latter
is confusing if the following types mismatch:

- the value type of `InputIt`
- the writable type(s) of `OutputIt`
- the types of the parameters of operator+ or `op`
- the return type of operator+ or `op`

`acc` serves as the intermediate object to store and provide the values for each
step of the computation:

- its type is the value type of `InputIt`
- it is written to `d_first`
- its value is passed to operator+ or `op`
- it stores the return value of operator+ or `op`

```cpp
enum not_int { x = 1, y = 2 };

char i_array[4] = {100, 100, 100, 100};
not_int e_array[4] = {x, x, y, y};
int  o_array[4];

// OK: uses operator+(char, char) and assigns char values to int array
std::partial_sum(i_array, i_array + 4, o_array);

// Error: cannot assign not_int values to int array
std::partial_sum(e_array, e_array + 4, o_array);

// OK: performs conversions when needed
// 1. creates `acc` of type char (the value type)
// 2. the char arguments are used for long multiplication (char -> long)
// 3. the long product is assigned to `acc` (long -> char)
// 4. `acc` is assigned to an element of `o_array` (char -> int)
// 5. go back to step 2 to process the remaining elements in the input range
std::partial_sum(i_array, i_array + 4, o_array, std::multiplies<long>{});
```

### Example

```cpp
#include <functional>
#include <iostream>
#include <iterator>
#include <numeric>
#include <vector>

int main()
{
    std::vector<int> v(10, 2); // v = {2, 2, 2, 2, 2, 2, 2, 2, 2, 2}

    std::cout << "The first " << v.size() << " even numbers are: ";
    // write the result to the cout stream
    std::partial_sum(v.cbegin(), v.cend(),
                     std::ostream_iterator<int>(std::cout, " "));
    std::cout << '\n';

    // write the result back to the vector v
    std::partial_sum(v.cbegin(), v.cend(),
                     v.begin(), std::multiplies<int>());

    std::cout << "The first " << v.size() << " powers of 2 are: ";
    for (int n : v)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

Output:

```text
The first 10 even numbers are: 2 4 6 8 10 12 14 16 18 20
The first 10 powers of 2 are: 2 4 8 16 32 64 128 256 512 1024
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

### See also

- **adjacent_difference** — computes the differences between adjacent elements
  in a range (function template)
- **accumulate** — sums up or folds a range of elements (function template)
- **inclusive_scan (C++17)** — similar to `std::partial_sum`, includes the ith
  input element in the ith sum (function template)
- **exclusive_scan (C++17)** — similar to `std::partial_sum`, excludes the ith
  input element from the ith sum (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/partial_sum*
