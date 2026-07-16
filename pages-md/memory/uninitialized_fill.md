# std::uninitialized_fill

```cpp
template< class ForwardIt, class T >
void uninitialized_fill( ForwardIt first, ForwardIt last, const T& value );  // (1)
template< class ExecutionPolicy, class ForwardIt, class T >
void uninitialized_fill( ExecutionPolicy&& policy,
                         ForwardIt first, ForwardIt last, const T& value );  // (2) (since C++17)
```

1) Copies the given `value` to an uninitialized memory area, defined by the
   range `[``first``,``last``)` as if by `for (; first != last; ++first) ::new
   (/* VOIDIFY */(*first)) typename
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

- **first, last** — the range of the elements to initialize
- **value** — the value to construct the elements with
- **policy** — the execution policy to use. See execution policy for details.

**Type requirements**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

**-No increment, assignment, comparison, or indirection through valid instances of `ForwardIt` may throw exceptions. Applying `&*` to a `ForwardIt` value must yield a pointer to its value type.(until C++11)**

### Return value

(none)

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
template<class ForwardIt, class T>
void uninitialized_fill(ForwardIt first, ForwardIt last, const T& value)
{
    using V = typename std::iterator_traits<ForwardIt>::value_type;
    ForwardIt current = first;
    try
    {
        for (; current != last; ++current)
            ::new (static_cast<void*>(std::addressof(*current))) V(value);
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

int main()
{
    const std::size_t sz = 4;
    std::allocator<std::string> alloc;
    std::string* p = alloc.allocate(sz);

    std::uninitialized_fill(p, p + sz, "Example");

    for (std::string* i = p; i != p + sz; ++i)
    {
        std::cout << *i << '\n';
        i->~basic_string<char>();
    }

    alloc.deallocate(p, sz);
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
  LWG 2433 | C++11 | this algorithm might be hijacked by overloaded operator& |
      uses `std::addressof`
  LWG 3870 | C++20 | this algorithm might create objects on a const storage |
      kept disallowed

### See also

- **uninitialized_fill_n** — copies an object to an uninitialized area of
  memory, defined by a start and a count (function template)
- **ranges::uninitialized_fill (C++20)** — copies an object to an uninitialized
  area of memory, defined by a range (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/memory/uninitialized_fill*
