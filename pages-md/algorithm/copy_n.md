# std::copy_n

```cpp
template< class InputIt, class Size, class OutputIt >
OutputIt copy_n( InputIt first, Size count, OutputIt result );  // (since C++11) (until C++20)
template< class InputIt, class Size, class OutputIt >
constexpr OutputIt copy_n( InputIt first, Size count, OutputIt result );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class Size, class ForwardIt2 >
ForwardIt2 copy_n( ExecutionPolicy&& policy,
                   ForwardIt1 first, Size count, ForwardIt2 result );  // (2) (since C++17)
```

1) Copies exactly `count` values from the range beginning at `first` to the
   range beginning at `result`. Formally, for each integer `0 ≤ i < count`,
   performs `*(result + i) = *(first + i)`. Overlap of ranges is formally
   permitted, but leads to unpredictable ordering of the results.

2) Same as (1), but executed according to `policy`. This overload does not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first** — the beginning of the range of elements to copy from
- **count** — number of the elements to copy
- **result** — the beginning of the destination range
- **policy** — the execution policy to use. See execution policy for details.

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-`OutputIt` must meet the requirements of LegacyOutputIterator.**

**-`ForwardIt1, ForwardIt2` must meet the requirements of LegacyForwardIterator.**

### Return value

Iterator in the destination range, pointing past the last element copied if
`count > 0` or `result` otherwise.

### Complexity

Zero assignments if `count < 0`; `count` assignments otherwise.

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
template<class InputIt, class Size, class OutputIt>
constexpr //< since C++20
OutputIt copy_n(InputIt first, Size count, OutputIt result)
{
    if (count > 0)
    {
        *result = *first;
        ++result;
        for (Size i = 1; i != count; ++i, ++result)
            *result = *++first;
    }

    return result;
}
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <numeric>
#include <string>
#include <vector>

int main()
{
    std::string in {"1234567890"};
    std::string out;

    std::copy_n(in.begin(), 4, std::back_inserter(out));
    std::cout << out << '\n';

    std::vector<int> v_in(128);
    std::iota(v_in.begin(), v_in.end(), 1);
    std::vector<int> v_out(v_in.size());

    std::copy_n(v_in.cbegin(), 100, v_out.begin());
    std::cout << std::accumulate(v_out.begin(), v_out.end(), 0) << '\n';
}
```

Output:

```text
1234
5050
```

### See also

- **copycopy_if (C++11)** — copies a range of elements to a new location
  (function template)
- **ranges::copy_n (C++20)** — copies a number of elements to a new location
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/copy_n*
