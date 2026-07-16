# std::replace_copy, std::replace_copy_if

```cpp
template< class InputIt, class OutputIt, class T >
OutputIt replace_copy( InputIt first, InputIt last, OutputIt d_first,
                       const T& old_value, const T& new_value );  // (until C++20)
template< class InputIt, class OutputIt, class T >
constexpr OutputIt replace_copy( InputIt first, InputIt last, OutputIt d_first,
                                 const T& old_value, const T& new_value );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2, class T >
ForwardIt2 replace_copy( ExecutionPolicy&& policy,
                         ForwardIt1 first, ForwardIt1 last, ForwardIt2 d_first,
                         const T& old_value, const T& new_value );  // (2) (since C++17)
template< class InputIt, class OutputIt, class UnaryPredicate, class T >
OutputIt replace_copy_if( InputIt first, InputIt last, OutputIt d_first,
                          UnaryPredicate p, const T& new_value );  // (until C++20)
template< class InputIt, class OutputIt, class UnaryPredicate, class T >
constexpr OutputIt replace_copy_if( InputIt first, InputIt last,
                                    OutputIt d_first,
                                    UnaryPredicate p, const T& new_value );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2,
          class UnaryPredicate, class T >
ForwardIt2 replace_copy_if( ExecutionPolicy&& policy,
                            ForwardIt1 first, ForwardIt1 last,
                            ForwardIt2 d_first,
                            UnaryPredicate p, const T& new_value );  // (4) (since C++17)
```

Copies the elements from the range `[``first``,``last``)` to another range
beginning at `d_first`, while replacing all elements satisfying specific
criteria with `new_value`. If the source and destination ranges overlap, the
behavior is undefined.

1) Replaces all elements that are equal to `old_value` (using `operator==`).

3) Replaces all elements for which predicate `p` returns `true`.

2,4) Same as (1,3), but executed according to `policy`. These overloads do not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first, last** — the range of elements to copy
- **d_first** — the beginning of the destination range
- **old_value** — the value of elements to replace
- **policy** — the execution policy to use. See execution policy for details.
- **p** — unary predicate which returns ​`true` if the element value should be
  replaced. The expression `p(v)` must be convertible to `bool` for every
  argument `v` of type (possibly const) `VT`, where `VT` is the value type of
  `InputIt`, regardless of value category, and must not modify `v`. Thus, a
  parameter type of `VT&`is not allowed, nor is `VT` unless for `VT` a move is
  equivalent to a copy(since C++11). ​
- **new_value** — the value to use as replacement

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-`OutputIt` must meet the requirements of LegacyOutputIterator.**

**-`ForwardIt1, ForwardIt2` must meet the requirements of LegacyForwardIterator.**

**-The results of the expressions `*first` and `new_value` must be writable to `d_first`.**

### Return value

Iterator to the element past the last element copied.

### Complexity

Given `N` as `std::distance(first, last)`:

1,2) exactly `N` comparisons with `old_value` using `operator==`.

3,4) exactly `N` applications of the predicate `p`.

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
template<class InputIt, class OutputIt, class T>
OutputIt replace_copy(InputIt first, InputIt last, OutputIt d_first,
                      const T& old_value, const T& new_value)
{
    for (; first != last; ++first)
        *d_first++ = (*first == old_value) ? new_value : *first;
    return d_first;
}
```

```cpp
template<class InputIt, class OutputIt, class UnaryPredicate, class T>
OutputIt replace_copy_if(InputIt first, InputIt last, OutputIt d_first,
                         UnaryPredicate p, const T& new_value)
{
    for (; first != last; ++first)
        *d_first++ = p(*first) ? new_value : *first;
    return d_first;
}
```

### Example

The following copy-prints a vector, replacing all values over `5` with `99` on
the fly.

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v{5, 7, 4, 2, 8, 6, 1, 9, 0, 3};
    std::replace_copy_if(v.begin(), v.end(),
                         std::ostream_iterator<int>(std::cout, " "),
                         [](int n){ return n > 5; }, 99);
    std::cout << '\n';
}
```

Output:

```text
5 99 4 2 99 99 1 99 0 3
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 283 | C++98 | `T` was required to be CopyAssignable (and
      EqualityComparable for `replace_copy`), but the value type of `InputIt` is
      not always `T` | removed the requirement
  LWG 337 | C++98 | `replace_copy_if` only required `InputIt` to meet the
      requirements of LegacyIterator[1] | corrected to LegacyInputIterator

1. The actual defect in the C++ standard is that the template parameter
   `InputIterator` was misspecified as `Iterator`. This affects the type
   requirements because the C++ standard states that for the function templates
   in the alrogithms library, the template type parameters whose name ends with
   `Iterator` imply the type requirements of the corresponding iterator
   categories.

### See also

- **replacereplace_if** — replaces all values satisfying specific criteria with
  another value (function template)
- **removeremove_if** — removes elements satisfying specific criteria (function
  template)
- **ranges::replace_copyranges::replace_copy_if (C++20)(C++20)** — copies a
  range, replacing elements satisfying specific criteria with another value
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/replace_copy*
