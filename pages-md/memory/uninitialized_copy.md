# std::uninitialized_copy

```cpp
template< class InputIt, class NoThrowForwardIt >
NoThrowForwardIt uninitialized_copy( InputIt first, InputIt last,
                                     NoThrowForwardIt d_first );  // (1)
template< class ExecutionPolicy, class ForwardIt, class NoThrowForwardIt >
NoThrowForwardIt uninitialized_copy( ExecutionPolicy&& policy,
                                     ForwardIt first, ForwardIt last,
                                     NoThrowForwardIt d_first );  // (2) (since C++17)
```

1) Copies elements from the range `[``first``,``last``)` to an uninitialized
   memory area beginning at `d_first` as if by `for (; first != last; ++d_first,
   (void) ++first) ::new (/* VOIDIFY */(*d_first)) typename
   std::iterator_traits<NoThrowForwardIt>::value_type(*first);`

where `/* VOIDIFY */(e)` is: `static_cast<void*>(&e)` (until C++11)
   `static_cast<void*>(std::addressof(e))` (since C++11)

If an exception is thrown during the initialization, the objects already
   constructed are destroyed in an unspecified order. If
   `d_first``+``[``​0​``,``std::distance(first, last)``)` overlaps with
   `[``first``,``last``)`, the behavior is undefined. (since C++20)

2) Same as (1), but executed according to `policy`. This overload does not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first, last** — the range of the elements to copy
- **d_first** — the beginning of the destination range
- **policy** — the execution policy to use. See execution policy for details.

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

**-`NoThrowForwardIt` must meet the requirements of LegacyForwardIterator.**

**-No increment, assignment, comparison, or indirection through valid instances of `NoThrowForwardIt` may throw exceptions. Applying `&*` to a `NoThrowForwardIt` value must yield a pointer to its value type.(until C++11)**

### Return value

Iterator to the element past the last element copied.

### Complexity

Linear in the distance between `first` and `last`.

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
template<class InputIt, class NoThrowForwardIt>
NoThrowForwardIt uninitialized_copy(InputIt first, InputIt last, NoThrowForwardIt d_first)
{
    using T = typename std::iterator_traits<NoThrowForwardIt>::value_type;
    NoThrowForwardIt current = d_first;
    try
    {
        for (; first != last; ++first, (void) ++current)
            ::new (static_cast<void*>(std::addressof(*current))) T(*first);
        return current;
    }
    catch (...)
    {
        for (; d_first != current; ++d_first)
            d_first->~T();
        throw;
    }
}
```

### Example

```cpp
#include <cstdlib>
#include <iostream>
#include <memory>
#include <string>

int main()
{
    const char *v[] = {"This", "is", "an", "example"};

    auto sz = std::size(v);

    if (void *pbuf = std::aligned_alloc(alignof(std::string), sizeof(std::string) * sz))
    {
        try
        {
            auto first = static_cast<std::string*>(pbuf);
            auto last = std::uninitialized_copy(std::begin(v), std::end(v), first);

            for (auto it = first; it != last; ++it)
                std::cout << *it << '_';
            std::cout << '\n';

            std::destroy(first, last);
        }
        catch (...) {}
        std::free(pbuf);
    }
}
```

Output:

```text
This_is_an_example_
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 866 | C++98 | given `T` as the value type of `NoThrowForwardIt`, if
      T::operator new exists, the program might be ill-formed | uses global
      replacement- new instead
  LWG 2433 | C++11 | this algorithm might be hijacked by overloaded operator& |
      uses `std::addressof`
  LWG 3870 | C++20 | this algorithm might create objects on a const storage |
      kept disallowed

### See also

- **uninitialized_copy_n (C++11)** — copies a number of objects to an
  uninitialized area of memory (function template)
- **ranges::uninitialized_copy (C++20)** — copies a range of objects to an
  uninitialized area of memory (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/memory/uninitialized_copy*
