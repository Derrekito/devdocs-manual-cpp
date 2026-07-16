# std::uninitialized_copy_n

```cpp
template< class InputIt, class Size, class NoThrowForwardIt >
NoThrowForwardIt uninitialized_copy_n( InputIt first, Size count,
                                       NoThrowForwardIt d_first );  // (1) (since C++11)
template< class ExecutionPolicy, class ForwardIt,
          class Size, class NoThrowForwardIt >
NoThrowForwardIt uninitialized_copy_n( ExecutionPolicy&& policy,
                                       ForwardIt first, Size count,
                                       NoThrowForwardIt d_first );  // (2) (since C++17)
```

1) Copies `count` elements from a range beginning at `first` to an uninitialized
   memory area beginning at `d_first` as if by `for (; n > 0; ++d_first, (void)
   ++first, --n) ::new (static_cast<void*>(std::addressof(*d_first))) typename
   std::iterator_traits<NoThrowForwardIt>::value_type(*first);`

If an exception is thrown during the initialization, the objects already
   constructed are destroyed in an unspecified order. If
   `d_first``+``[``​0​``,``n``)` overlaps with `first``+``[``​0​``,``n``)`, the
   behavior is undefined. (since C++20)

2) Same as (1), but executed according to `policy`. This overload does not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first** — the beginning of the range of the elements to copy
- **count** — the number of elements to copy
- **d_first** — the beginning of the destination range
- **policy** — the execution policy to use. See execution policy for details.

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

**-`NoThrowForwardIt` must meet the requirements of LegacyForwardIterator.**

**-No increment, assignment, comparison, or indirection through valid instances of `NoThrowForwardIt` may throw exceptions.**

### Return value

Iterator to the element past the last element copied.

### Complexity

Linear in `count`.

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
template<class InputIt, class Size, class NoThrowForwardIt>
NoThrowForwardIt uninitialized_copy_n(InputIt first, Size count, NoThrowForwardIt d_first)
{
    using T = typename std::iterator_traits<NoThrowForwardIt>::value_type;
    NoThrowForwardIt current = d_first;
    try
    {
        for (; count > 0; ++first, (void) ++current, --count)
            ::new (static_cast<void*>(std::addressof(*current))) T(*first);
    }
    catch (...)
    {
        for (; d_first != current; ++d_first)
            d_first->~T();
        throw;
    }
    return current;
}
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <memory>
#include <string>
#include <tuple>
#include <vector>

int main()
{
    std::vector<std::string> v = {"This", "is", "an", "example"};

    std::string* p;
    std::size_t sz;
    std::tie(p, sz) = std::get_temporary_buffer<std::string>(v.size());
    sz = std::min(sz, v.size());

    std::uninitialized_copy_n(v.begin(), sz, p);

    for (std::string* i = p; i != p + sz; ++i)
    {
        std::cout << *i << ' ';
        i->~basic_string<char>();
    }
    std::cout << '\n';

    std::return_temporary_buffer(p);
}
```

Possible output:

```text
This is an example
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2433 | C++11 | this algorithm might be hijacked by overloaded operator& |
      uses `std::addressof`
  LWG 3870 | C++20 | this algorithm might create objects on a const storage |
      kept disallowed

### See also

- **uninitialized_copy** — copies a range of objects to an uninitialized area of
  memory (function template)
- **ranges::uninitialized_copy_n (C++20)** — copies a number of objects to an
  uninitialized area of memory (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/memory/uninitialized_copy_n*
