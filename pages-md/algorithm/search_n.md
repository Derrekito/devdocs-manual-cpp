# std::search_n

```cpp
template< class ForwardIt, class Size, class T >
ForwardIt search_n( ForwardIt first, ForwardIt last,
                    Size count, const T& value );  // (until C++20)
template< class ForwardIt, class Size, class T >
constexpr ForwardIt search_n( ForwardIt first, ForwardIt last,
                              Size count, const T& value );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class Size, class T >
ForwardIt search_n( ExecutionPolicy&& policy, ForwardIt first, ForwardIt last,
                    Size count, const T& value );  // (2) (since C++17)
template< class ForwardIt, class Size, class T, class BinaryPredicate >
ForwardIt search_n( ForwardIt first, ForwardIt last,
                    Size count, const T& value, BinaryPredicate p );  // (until C++20)
template< class ForwardIt, class Size, class T, class BinaryPredicate >
constexpr ForwardIt search_n( ForwardIt first, ForwardIt last,
                              Size count, const T& value, BinaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt,
          class Size, class T, class BinaryPredicate >
ForwardIt search_n( ExecutionPolicy&& policy, ForwardIt first, ForwardIt last,
                    Size count, const T& value, BinaryPredicate p );  // (4) (since C++17)
```

Searches the range `[``first``,``last``)` for the first sequence of `count`
identical elements, each equal to the given `value`.

1) Elements are compared using `operator==`.

3) Elements are compared using the given binary predicate `p`.

2,4) Same as (1,3), but executed according to `policy`. These overloads do not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first, last** — the range of elements to examine
- **count** — the length of the sequence to search for
- **value** — the value of the elements to search for
- **policy** — the execution policy to use. See execution policy for details.
- **p** — binary predicate which returns ​`true` if the elements should be
  treated as equal. The signature of the predicate function should be equivalent
  to the following: `bool pred(const Type1 &a, const Type2 &b);` While the
  signature does not need to have `const &`, the function must not modify the
  objects passed to it and must be able to accept all values of type (possibly
  const) `Type1` and `Type2` regardless of value category (thus, `Type1 &` is
  not allowed, nor is `Type1` unless for `Type1` a move is equivalent to a
  copy(since C++11)). The type Type1 must be such that an object of type
  ForwardIt can be dereferenced and then implicitly converted to Type1. The type
  Type2 must be such that an object of type T can be implicitly converted to
  Type2. ​

**Type requirements**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

**-`Size` must be convertible to integral type.**

### Return value

If `count` is positive, returns an iterator to the beginning of the first
sequence found in the range `[``first``,``last``)`. Each iterator `it` in the
sequence should satisfy the following condition:

1,2) `*it == value` is `true`.

3,4) `p(*it, value) != false` is `true`.

If no such sequence is found, `last` is returned.

If `count` is zero or negative, `first` is returned.

### Complexity

Given `N` as `std::distance(first, last)`:

1,2) at most `N` comparisons with value using `operator==`.

3,4) at most `N` applications of the predicate `p`.

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
template<class ForwardIt, class Size, class T>
ForwardIt search_n(ForwardIt first, ForwardIt last, Size count, const T& value)
{
    if (count <= 0)
        return first;

    for (; first != last; ++first)
    {
        if (!(*first == value))
            continue;

        ForwardIt candidate = first;

        for (Size cur_count = 1; true; ++cur_count)
        {
            if (cur_count >= count)
                return candidate; // success

            ++first;
            if (first == last)
                return last; // exhausted the list

            if (!(*first == value))
                break; // too few in a row
        }
    }
    return last;
}
```

```cpp
template<class ForwardIt, class Size, class T, class BinaryPredicate>
ForwardIt search_n(ForwardIt first, ForwardIt last, Size count, const T& value,
                   BinaryPredicate p)
{
    if (count <= 0)
        return first;

    for (; first != last; ++first)
    {
        if (!p(*first, value))
            continue;

        ForwardIt candidate = first;

        for (Size cur_count = 1; true; ++cur_count)
        {
            if (cur_count >= count)
                return candidate; // success

            ++first;
            if (first == last)
                return last; // exhausted the list

            if (!p(*first, value))
                break; // too few in a row
        }
    }
    return last;
}
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>

template<class Container, class Size, class T>
[[nodiscard]]
constexpr bool consecutive_values(const Container& c, Size count, const T& v)
{
    return std::search_n(std::begin(c), std::end(c), count, v) != std::end(c);
}

int main()
{
    constexpr char sequence[] = "1001010100010101001010101";

    static_assert(consecutive_values(sequence, 3, '0'));

    std::cout << std::boolalpha
              << "Has 4 consecutive zeros: "
              << consecutive_values(sequence, 4, '0') << '\n'
              << "Has 3 consecutive zeros: "
              << consecutive_values(sequence, 3, '0') << '\n';
}
```

Output:

```text
Has 4 consecutive zeros: false
Has 3 consecutive zeros: true
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 283 | C++98 | `T` was required to be EqualityComparable, but the value
      type of `InputIt` is not always `T` | removed the requirement
  LWG 426 | C++98 | the complexity upper limit was `N·count`, it is negative if
      `count` is negative | the upper limit is `​0​` if `count` is non-positive
  LWG 714 | C++98 | if `count > 0`, the complexity upper limit was `N·count`,
      but in the worst case the number of comparisons/operations is always `N` |
      changed the upper limit to `N` in this case

### See also

- **find_end** — finds the last sequence of elements in a certain range
  (function template)
- **findfind_iffind_if_not (C++11)** — finds the first element satisfying
  specific criteria (function template)
- **search** — searches for a range of elements (function template)
- **ranges::search_n (C++20)** — searches for a number consecutive copies of an
  element in a range (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/search_n*
