# std::find, std::find_if, std::find_if_not

```cpp
template< class InputIt, class T >
InputIt find( InputIt first, InputIt last, const T& value );  // (until C++20)
template< class InputIt, class T >
constexpr InputIt find( InputIt first, InputIt last, const T& value );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class T >
ForwardIt find( ExecutionPolicy&& policy,
                ForwardIt first, ForwardIt last, const T& value );  // (2) (since C++17)
template< class InputIt, class UnaryPredicate >
InputIt find_if( InputIt first, InputIt last, UnaryPredicate p );  // (until C++20)
template< class InputIt, class UnaryPredicate >
constexpr InputIt find_if( InputIt first, InputIt last, UnaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class UnaryPredicate >
ForwardIt find_if( ExecutionPolicy&& policy,
                   ForwardIt first, ForwardIt last, UnaryPredicate p );  // (4) (since C++17)
template< class InputIt, class UnaryPredicate >
InputIt find_if_not( InputIt first, InputIt last, UnaryPredicate q );  // (since C++11) (until C++20)
template< class InputIt, class UnaryPredicate >
constexpr InputIt find_if_not( InputIt first, InputIt last, UnaryPredicate q );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class UnaryPredicate >
ForwardIt find_if_not( ExecutionPolicy&& policy,
                       ForwardIt first, ForwardIt last, UnaryPredicate q );  // (6) (since C++17)
```

Returns an iterator to the first element in the range `[``first``,``last``)`
that satisfies specific criteria (or `last` if there is no such iterator):

1) `find` searches for an element equal to `value` (using `operator==`).

3) `find_if` searches for an element for which predicate `p` returns `true`.

5) `find_if_not` searches for an element for which predicate `q` returns
   `false`.

2,4,6) Same as (1,3,5), but executed according to `policy`. These overloads do
   not participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first, last** — the range of elements to examine
- **value** — value to compare the elements to
- **policy** — the execution policy to use. See execution policy for details.
- **p** — unary predicate which returns ​`true` for the required element. The
  expression `p(v)` must be convertible to `bool` for every argument `v` of type
  (possibly const) `VT`, where `VT` is the value type of `InputIt`, regardless
  of value category, and must not modify `v`. Thus, a parameter type of `VT&`is
  not allowed, nor is `VT` unless for `VT` a move is equivalent to a copy(since
  C++11). ​
- **q** — unary predicate which returns ​`false` for the required element. The
  expression `q(v)` must be convertible to `bool` for every argument `v` of type
  (possibly const) `VT`, where `VT` is the value type of `InputIt`, regardless
  of value category, and must not modify `v`. Thus, a parameter type of `VT&`is
  not allowed, nor is `VT` unless for `VT` a move is equivalent to a copy(since
  C++11). ​

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

**-`UnaryPredicate` must meet the requirements of Predicate.**

### Return value

The first iterator `it` in the range `[``first``,``last``)` satisfying the
following condition or `last` if there is no such iterator:

1,2) `*it == value` is `true`.

3,4) `p(*it)` is `true`.

5,6) `q(*it)` is `false`.

### Complexity

Given `N` as `std::distance(first, last)`:

1,2) At most `N` comparisons with `value` using `operator==`.

3,4) At most `N` applications of the predicate `p`.

5,6) At most `N` applications of the predicate `q`.

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
template<class InputIt, class T>
constexpr InputIt find(InputIt first, InputIt last, const T& value)
{
    for (; first != last; ++first)
        if (*first == value)
            return first;

    return last;
}
```

```cpp
template<class InputIt, class UnaryPredicate>
constexpr InputIt find_if(InputIt first, InputIt last, UnaryPredicate p)
{
    for (; first != last; ++first)
        if (p(*first))
            return first;

    return last;
}
```

```cpp
template<class InputIt, class UnaryPredicate>
constexpr InputIt find_if_not(InputIt first, InputIt last, UnaryPredicate q)
{
    for (; first != last; ++first)
        if (!q(*first))
            return first;

    return last;
}
```

### Notes

If you do not have C++11, an equivalent to `std::find_if_not` is to use
`std::find_if` with the negated predicate.

```cpp
template<class InputIt, class UnaryPredicate>
InputIt find_if_not(InputIt first, InputIt last, UnaryPredicate q)
{
    return std::find_if(first, last, std::not1(q));
}
```

### Example

The following example finds integers in given `std::vector`.

```cpp
#include <algorithm>
#include <array>
#include <iostream>

int main()
{
    const auto v = {1, 2, 3, 4};

    for (const int n : {3, 5})
        (std::find(v.begin(), v.end(), n) == std::end(v))
            ? std::cout << "v does not contain " << n << '\n'
            : std::cout << "v contains " << n << '\n';

    auto is_even = [](int i) { return i % 2 == 0; };

    for (auto const& w : {std::array{3, 1, 4}, {1, 3, 5}})
        if (auto it = std::find_if(begin(w), end(w), is_even); it != std::end(w))
            std::cout << "w contains an even number " << *it << '\n';
        else
            std::cout << "w does not contain even numbers\n";
}
```

Output:

```text
v contains 3
v does not contain 5
w contains an even number 4
w does not contain even numbers
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 283 | C++98 | `T` was required to be EqualityComparable, but the value
      type of `InputIt` is not always `T` | removed the requirement

### See also

- **adjacent_find** — finds the first two adjacent items that are equal (or
  satisfy a given predicate) (function template)
- **find_end** — finds the last sequence of elements in a certain range
  (function template)
- **find_first_of** — searches for any one of a set of elements (function
  template)
- **mismatch** — finds the first position where two ranges differ (function
  template)
- **search** — searches for a range of elements (function template)
- **ranges::findranges::find_ifranges::find_if_not (C++20)(C++20)(C++20)** —
  finds the first element satisfying specific criteria (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/find*
