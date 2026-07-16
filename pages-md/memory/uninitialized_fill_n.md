# std::uninitialized_fill_n

```cpp
template< class ForwardIt, class Size, class T >
ForwardIt uninitialized_fill_n( ForwardIt first, Size count, const T& value );  // (1)
template< class ExecutionPolicy, class ForwardIt, class Size, class T >
ForwardIt uninitialized_fill_n( ExecutionPolicy&& policy,
                                ForwardIt first, Size count, const T& value );  // (2) (since C++17)
```

1) Copies the given value `value` to the first `count` elements in an
   uninitialized memory area beginning at `first` as if by `for (; n--; ++first)
   ::new (/* VOIDIFY */(*first)) typename
   std::iterator_traits<ForwardIt>::value_type(value);`

where `/* VOIDIFY */(e)` is: `static_cast<void*>(&e)` (until C++11)
   `static_cast<void*>(std::addressof(e))` (since C++11)

If an exception is thrown during the initialization, the objects already
   constructed are destroyed in an unspecified order.

2) Same as (1), but executed according to `policy`. This overload does not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first** — the beginning of the range of the elements to initialize
- **count** — number of elements to construct
- **value** — the value to construct the elements with

**Type requirements**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

**-No increment, assignment, comparison, or indirection through valid instances of `ForwardIt` may throw exceptions. Applying `&*` to a `ForwardIt` value must yield a pointer to its value type.(until C++11)**

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
template<class ForwardIt, class Size, class T>
ForwardIt uninitialized_fill_n(ForwardIt first, Size count, const T& value)
{
    using V = typename std::iterator_traits<ForwardIt>::value_type;
    ForwardIt current = first;
    try
    {
        for (; count > 0; ++current, (void) --count)
            ::new (static_cast<void*>(std::addressof(*current))) V(value);
        return current;
    }
    catch (...)
    {
        for (; first != current; ++first)
            first->~V();
        throw;
    }
}
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <memory>
#include <string>
#include <tuple>

int main()
{
    std::string* p;
    std::size_t sz;
    std::tie(p, sz) = std::get_temporary_buffer<std::string>(4);
    std::uninitialized_fill_n(p, sz, "Example");

    for (std::string* i = p; i != p + sz; ++i)
    {
        std::cout << *i << '\n';
        i->~basic_string<char>();
    }
    std::return_temporary_buffer(p);
}
```

Output:

```text
Example
Example
Example
Example
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 866 | C++98 | given `T` as the value type of `ForwardIt`, if T::operator
      new exists, the program might be ill-formed | uses global replacement- new
      instead
  LWG 1339 | C++98 | the location of the first element following the filling
      range was not returned | returned
  LWG 2433 | C++11 | this algorithm might be hijacked by overloaded `operator&`
      | uses `std::addressof`
  LWG 3870 | C++20 | this algorithm might create objects on a `const` storage |
      kept disallowed

### See also

- **uninitialized_fill** — copies an object to an uninitialized area of memory,
  defined by a range (function template)
- **ranges::uninitialized_fill_n (C++20)** — copies an object to an
  uninitialized area of memory, defined by a start and a count (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/memory/uninitialized_fill_n*
