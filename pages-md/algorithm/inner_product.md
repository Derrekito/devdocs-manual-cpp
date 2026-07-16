# std::inner_product

```cpp
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

Computes inner product (i.e. sum of products) or performs ordered map/reduce
operation on the range `[``first1``,``last1``)` and the range beginning at
`first2`.

1) Initializes the accumulator `acc` (of type `T`) with the initial value `init`
   and then modifies it with the expression `acc = acc + (*i1) * (*i2)`(until
   C++11)`acc = std::move(acc) + (*i1) * (*i2)`(since C++11) for every iterator
   `i1` in the range `[``first1``,``last1``)` in order and its corresponding
   iterator `i2` in the range beginning at `first2`. For built-in meaning of +
   and *, this computes inner product of the two ranges.

2) Initializes the accumulator `acc` (of type `T`) with the initial value `init`
   and then modifies it with the expression `acc = op1(acc, op2(*i1,
   *i2))`(until C++11)`acc = op1(std::move(acc), op2(*i1, *i2))`(since C++11)
   for every iterator `i1` in the range `[``first1``,``last1``)` in order and
   its corresponding iterator `i2` in the range beginning at `first2`.

If `op1` or `op2` invalidates any iterators (including the end iterators) or
modify any elements of the range involved, the behavior is undefined.

### Parameters

- **first1, last1** — the first range of elements
- **first2** — the beginning of the second range of elements
- **init** — initial value of the sum of the products
- **op1** — binary operation function object that will be applied. This "sum"
  function takes a value returned by `op2` and the current value of the
  accumulator and produces a new value to be stored in the accumulator. The
  signature of the function should be equivalent to the following: `Ret
  fun(const Type1 &a, const Type2 &b);` The signature does not need to have
  `const &`. The types Type1 and Type2 must be such that objects of types T and
  Type3 can be implicitly converted to Type1 and Type2 respectively. The type
  `Ret` must be such that an object of type `T` can be assigned a value of type
  `Ret`. ​
- **op2** — binary operation function object that will be applied. This
  "product" function takes one value from each range and produces a new value.
  The signature of the function should be equivalent to the following: `Ret
  fun(const Type1 &a, const Type2 &b);` The signature does not need to have
  `const &`. The types Type1 and Type2 must be such that objects of types
  InputIt1 and InputIt2 can be dereferenced and then implicitly converted to
  Type1 and Type2 respectively. The type `Ret` must be such that an object of
  type `Type3` can be assigned a value of type `Ret`. ​

**Type requirements**

**-`InputIt1, InputIt2` must meet the requirements of LegacyInputIterator.**

**-`ForwardIt1, ForwardIt2` must meet the requirements of LegacyForwardIterator.**

**-`T` must meet the requirements of CopyAssignable and CopyConstructible.**

### Return value

`acc` after all modifications.

### Possible implementation

```cpp
template<class InputIt1, class InputIt2, class T>
constexpr // since C++20
T inner_product(InputIt1 first1, InputIt1 last1, InputIt2 first2, T init)
{
    while (first1 != last1)
    {
        init = std::move(init) + (*first1) * (*first2); // std::move since C++11
        ++first1;
        ++first2;
    }

    return init;
}
```

```cpp
template<class InputIt1, class InputIt2, class T,
         class BinaryOperation1, class BinaryOperation2>
constexpr // since C++20
T inner_product(InputIt1 first1, InputIt1 last1,
                InputIt2 first2, T init,
                BinaryOperation1 op1
                BinaryOperation2 op2)
{
    while (first1 != last1)
    {
        init = op1(std::move(init), op2(*first1, *first2)); // std::move since C++11
        ++first1;
        ++first2;
    }

    return init;
}
```

### Notes

The parallelizable version of this algorithm, `std::transform_reduce`, requires
`op1` and `op2` to be commutative and associative, but `std::inner_product`
makes no such requirement, and always performs the operations in the order
given.

### Example

```cpp
#include <functional>
#include <iostream>
#include <numeric>
#include <vector>

int main()
{
    std::vector<int> a {0, 1, 2, 3, 4};
    std::vector<int> b {5, 4, 2, 3, 1};

    int r1 = std::inner_product(a.begin(), a.end(), b.begin(), 0);
    std::cout << "Inner product of a and b: " << r1 << '\n';

    int r2 = std::inner_product(a.begin(), a.end(), b.begin(), 0,
                                std::plus<>(), std::equal_to<>());
    std::cout << "Number of pairwise matches between a and b: " <<  r2 << '\n';
}
```

Output:

```text
Inner product of a and b: 21
Number of pairwise matches between a and b: 2
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 242 | C++98 | `op1` and `op2` could not have side effects | they cannot
      modify the ranges involved
  LWG 2055 (P0616R0) | C++11 | `acc` was not moved while being accumulated | it
      is moved

### See also

- **transform_reduce (C++17)** — applies an invocable, then reduces out of order
  (function template)
- **accumulate** — sums up or folds a range of elements (function template)
- **partial_sum** — computes the partial sum of a range of elements (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/inner_product*
