# std::fill_n

```cpp
template< class OutputIt, class Size, class T >
OutputIt fill_n( OutputIt first, Size count, const T& value );  // (until C++20)
template< class OutputIt, class Size, class T >
constexpr OutputIt fill_n( OutputIt first, Size count, const T& value );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class Size, class T >
ForwardIt fill_n( ExecutionPolicy&& policy,
                  ForwardIt first, Size count, const T& value );  // (2) (since C++17)
```

1) Assigns the given `value` to the first `count` elements in the range
   beginning at `first` if `count > 0`. Does nothing otherwise.

2) Same as (1), but executed according to `policy`. This overload does not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first** — the beginning of the range of elements to modify
- **count** — number of elements to modify
- **value** — the value to be assigned
- **policy** — the execution policy to use. See execution policy for details.

**Type requirements**

**-`OutputIt` must meet the requirements of LegacyOutputIterator.**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

**-`value` must be writable to `first`.**

**-`Size` must be convertible to integral type.**

### Return value

Iterator one past the last element assigned if `count > 0`, `first` otherwise.

### Complexity

Exactly `std::max(0, count)` assignments.

### Exceptions

The overload with a template parameter named `ExecutionPolicy` reports errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Possible implementation

```cpp
template<class OutputIt, class Size, class T>
OutputIt fill_n(OutputIt first, Size count, const T& value)
{
    for (Size i = 0; i < count; i++)
        *first++ = value;
    return first;
}
```

### Example

The following code uses `fill_n()` to assign `-1` to the first half of a vector
of integers:

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v1{0, 1, 2, 3, 4, 5, 6, 7, 8, 9};

    std::fill_n(v1.begin(), 5, -1);

    std::copy(begin(v1), end(v1), std::ostream_iterator<int>(std::cout, " "));
    std::cout << '\n';
}
```

Output:

```text
-1 -1 -1 -1 -1 5 6 7 8 9
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 283 | C++98 | `T` was required to be CopyAssignable, but `T` is not always
      writable to `OutputIt` | required to be writable instead
  LWG 426 | C++98 | the complexity requirement was 'exactly `count`
      assignments', which is broken if `count` is negative | no assignment if
      `count` is non-positive
  LWG 865 | C++98 | the location of the first element following the filling
      range was not returned | returned

### See also

- **fill** — copy-assigns the given value to every element in a range (function
  template)
- **ranges::fill_n (C++20)** — assigns a value to a number of elements
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/fill_n*
