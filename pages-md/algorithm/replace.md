# std::replace, std::replace_if

```cpp
template< class ForwardIt, class T >
void replace( ForwardIt first, ForwardIt last,
              const T& old_value, const T& new_value );  // (until C++20)
template< class ForwardIt, class T >
constexpr void replace( ForwardIt first, ForwardIt last,
                        const T& old_value, const T& new_value );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class T >
void replace( ExecutionPolicy&& policy, ForwardIt first, ForwardIt last,
              const T& old_value, const T& new_value );  // (2) (since C++17)
template< class ForwardIt, class UnaryPredicate, class T >
void replace_if( ForwardIt first, ForwardIt last,
                 UnaryPredicate p, const T& new_value );  // (until C++20)
template< class ForwardIt, class UnaryPredicate, class T >
constexpr void replace_if( ForwardIt first, ForwardIt last,
                           UnaryPredicate p, const T& new_value );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt,
          class UnaryPredicate, class T >
void replace_if( ExecutionPolicy&& policy, ForwardIt first, ForwardIt last,
                 UnaryPredicate p, const T& new_value );  // (4) (since C++17)
```

Replaces all elements satisfying specific criteria with `new_value` in the range
`[``first``,``last``)`.

1) Replaces all elements that are equal to `old_value` (using `operator==`).

3) Replaces all elements for which predicate `p` returns `true`.

2,4) Same as (1,3), but executed according to `policy`. These overloads do not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first, last** — the range of elements to process
- **old_value** — the value of elements to replace
- **policy** — the execution policy to use. See execution policy for details.
- **p** — unary predicate which returns ​`true` if the element value should be
  replaced. The expression `p(v)` must be convertible to `bool` for every
  argument `v` of type (possibly const) `VT`, where `VT` is the value type of
  `ForwardIt`, regardless of value category, and must not modify `v`. Thus, a
  parameter type of `VT&`is not allowed, nor is `VT` unless for `VT` a move is
  equivalent to a copy(since C++11). ​
- **new_value** — the value to use as replacement

**Type requirements**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

**-`UnaryPredicate` must meet the requirements of Predicate.**

The expression `*first = new_value` must be valid.
*(until C++20)*

`new_value` must be writable to `first`.
*(since C++20)*

### Return value

(none)

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

### Notes

Because the algorithm takes `old_value` and `new_value` by reference, it can
have unexpected behavior if either is a reference to an element of the range
`[``first``,``last``)`.

### Possible implementation

```cpp
template<class ForwardIt, class T>
void replace(ForwardIt first, ForwardIt last,
             const T& old_value, const T& new_value)
{
    for (; first != last; ++first)
        if (*first == old_value)
            *first = new_value;
}
```

```cpp
template<class ForwardIt, class UnaryPredicate, class T>
void replace_if(ForwardIt first, ForwardIt last,
                UnaryPredicate p, const T& new_value)
{
    for (; first != last; ++first)
        if (p(*first))
            *first = new_value;
}
```

### Example

The following code at first replaces all occurrences of `8` with `88` in a
vector of integers. Then it replaces all values less than `5` with `55`.

```cpp
#include <algorithm>
#include <array>
#include <functional>
#include <iostream>

int main()
{
    std::array<int, 10> s{5, 7, 4, 2, 8, 6, 1, 9, 0, 3};

    std::replace(s.begin(), s.end(), 8, 88);

    for (int a : s)
        std::cout << a << ' ';
    std::cout << '\n';

    std::replace_if(s.begin(), s.end(),
                    std::bind(std::less<int>(), std::placeholders::_1, 5), 55);

    for (int a : s)
        std::cout << a << ' ';
    std::cout << '\n';
}
```

Output:

```text
5 7 4 2 88 6 1 9 0 3
5 7 55 55 88 6 55 9 55 55
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 283 | C++98 | `T` was required to be CopyAssignable (and
      EqualityComparable for `replace`), but the value type of `ForwardIt` is
      not always `T` and `T` is not always writable to `ForwardIt` | required
      `*first = new_value` to be valid instead

### See also

- **replace_copyreplace_copy_if** — copies a range, replacing elements
  satisfying specific criteria with another value (function template)
- **ranges::replaceranges::replace_if (C++20)(C++20)** — replaces all values
  satisfying specific criteria with another value (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/replace*
