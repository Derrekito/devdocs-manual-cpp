# std::fill

```cpp
template< class ForwardIt, class T >
void fill( ForwardIt first, ForwardIt last, const T& value );  // (until C++20)
template< class ForwardIt, class T >
constexpr void fill( ForwardIt first, ForwardIt last, const T& value );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class T >
void fill( ExecutionPolicy&& policy,
           ForwardIt first, ForwardIt last, const T& value );  // (2) (since C++17)
```

1) Assigns the given `value` to the elements in the range
   `[``first``,``last``)`.

2) Same as (1), but executed according to `policy`. This overload does not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first, last** — the range of elements to modify
- **value** — the value to be assigned
- **policy** — the execution policy to use. See execution policy for details.

**Type requirements**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

**-`value` must be writable to `first`.**

### Return value

(none)

### Complexity

Exactly `std::distance(first, last)` assignments.

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
template<class ForwardIt, class T>
void fill(ForwardIt first, ForwardIt last, const T& value)
{
    for (; first != last; ++first)
        *first = value;
}
```

### Example

The following code uses `fill()` to set all of the elements of a
`std::vector<int>` to `-1`:

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};

    std::fill(v.begin(), v.end(), -1);

    for (auto elem : v)
        std::cout << elem << ' ';
    std::cout << '\n';
}
```

Output:

```text
-1 -1 -1 -1 -1 -1 -1 -1 -1 -1
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 283 | C++98 | `T` was required to be CopyAssignable, but `T` is not always
      writable to `ForwardIt` | required to be writable instead

### See also

- **fill_n** — copy-assigns the given value to N elements in a range (function
  template)
- **copycopy_if (C++11)** — copies a range of elements to a new location
  (function template)
- **generate** — assigns the results of successive function calls to every
  element in a range (function template)
- **transform** — applies a function to a range of elements, storing results in
  a destination range (function template)
- **ranges::fill (C++20)** — assigns a range of elements a certain value
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/fill*
