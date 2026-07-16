# std::for_each_n

```cpp
template< class InputIt, class Size, class UnaryFunction >
InputIt for_each_n( InputIt first, Size n, UnaryFunction f );  // (since C++17) (until C++20)
template< class InputIt, class Size, class UnaryFunction >
constexpr InputIt for_each_n( InputIt first, Size n, UnaryFunction f );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class Size, class UnaryFunction2 >
ForwardIt for_each_n( ExecutionPolicy&& policy,
                      ForwardIt first, Size n, UnaryFunction2 f );  // (2) (since C++17)
```

1) Applies the given function object `f` to the result of dereferencing every
   iterator in the range `[``first``,``first + n``)`, in order.

2) Applies the given function object `f` to the result of dereferencing every
   iterator in the range `[``first``,``first + n``)` (not necessarily in order).
   The algorithm is executed according to `policy`. This overload does not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

For both overloads, if the iterator type is mutable, `f` may modify the elements
of the range through the dereferenced iterator. If `f` returns a result, the
result is ignored. If `n` is less than zero, the behavior is undefined.

Unlike the rest of the parallel algorithms, `for_each_n` is not allowed to make
copies of the elements in the sequence even if they are trivially copyable.

### Parameters

- **first** — the beginning of the range to apply the function to
- **n** — the number of elements to apply the function to
- **policy** — the execution policy to use. See execution policy for details.
- **f** — function object, to be applied to the result of dereferencing every
  iterator in the range `[``first``,``first + n``)` The signature of the
  function should be equivalent to the following: `void fun(const Type &a);` The
  signature does not need to have `const &`. The type Type must be such that an
  object of type InputIt can be dereferenced and then implicitly converted to
  Type. ​

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

**-`UnaryFunction` must meet the requirements of MoveConstructible. Does not have to be CopyConstructible.**

**-`UnaryFunction2` must meet the requirements of CopyConstructible.**

### Return value

An iterator equal to `first + n`, or more formally, to `std::advance(first, n)`.

### Complexity

Exactly `n` applications of `f`.

### Exceptions

The overload with a template parameter named `ExecutionPolicy` reports errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Possible implementation

See also the implementation in libstdc++, libc++ and MSVC stdlib.

```cpp
template<class InputIt, class Size, class UnaryFunction>
InputIt for_each_n(InputIt first, Size n, UnaryFunction f)
{
    for (Size i = 0; i < n; ++first, (void) ++i)
        f(*first);

    return first;
}
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

void println(auto const& v)
{
    for (auto count{v.size()}; auto const& e : v)
        std::cout << e << (--count ? ", " : "\n");
}

int main()
{
    std::vector<int> vi {1, 2, 3, 4, 5};
    println(vi);

    std::for_each_n(vi.begin(), 3, [](auto& n) { n *= 2; });
    println(vi);
}
```

Output:

```text
1, 2, 3, 4, 5
2, 4, 6, 4, 5
```

### See also

- **transform** — applies a function to a range of elements, storing results in
  a destination range (function template)
- **range-`for` loop(C++11)** — executes loop over range
- **for_each** — applies a function to a range of elements (function template)
- **ranges::for_each_n (C++20)** — applies a function object to the first N
  elements of a sequence (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/for_each_n*
