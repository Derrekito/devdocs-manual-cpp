# std::uninitialized_default_construct

```cpp
template< class ForwardIt >
void uninitialized_default_construct( ForwardIt first, ForwardIt last );  // (1) (since C++17)
template< class ExecutionPolicy, class ForwardIt >
void uninitialized_default_construct( ExecutionPolicy&& policy,
                                      ForwardIt first, ForwardIt last );  // (2) (since C++17)
```

1) Constructs objects of type `typename
   std::iterator_traits<ForwardIt>::value_type` in the uninitialized storage
   designated by the range `[``first``,``last``)` by default-initialization, as
   if by `for (; first != last; ++first) ::new
   (static_cast<void*>(std::addressof(*first))) typename
   std::iterator_traits<ForwardIt>::value_type;`

If an exception is thrown during the initialization, the objects already
   constructed are destroyed in an unspecified order.

2) Same as (1), but executed according to `policy`. This overload does not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first, last** — the range of the elements to initialize
- **policy** — the execution policy to use. See execution policy for details.

**Type requirements**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

**-No increment, assignment, comparison, or indirection through valid instances of `ForwardIt` may throw exceptions.**

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
template<class ForwardIt>
void uninitialized_default_construct(ForwardIt first, ForwardIt last)
{
    using Value = typename std::iterator_traits<ForwardIt>::value_type;
    ForwardIt current = first;
    try
    {
        for (; current != last; ++current)
        {
            ::new (const_cast<void*>(static_cast<const volatile void*>(
                std::addressof(*current)))) Value;
        }
    }
    catch (...)
    {
        std::destroy(first, current);
        throw;
    }
}
```

### Example

```cpp
#include <cstring>
#include <iostream>
#include <memory>
#include <string>

struct S
{
    std::string m{"Default value"};
};

int main()
{
    constexpr int n{3};
    alignas(alignof(S)) unsigned char mem[n * sizeof(S)];

    try
    {
        auto first{reinterpret_cast<S*>(mem)};
        auto last{first + n};

        std::uninitialized_default_construct(first, last);

        for (auto it{first}; it != last; ++it)
            std::cout << it->m << '\n';

        std::destroy(first, last);
    }
    catch (...)
    {
        std::cout << "Exception!\n";
    }

    // Notice that for "trivial types" the uninitialized_default_construct
    // generally does not zero-fill the given uninitialized memory area.
    int v[]{1, 2, 3, 4};
    const int original[]{1, 2, 3, 4};
    std::uninitialized_default_construct(std::begin(v), std::end(v));

    // Maybe undefined behavior, pending CWG 1997.
    // for (const int i : v)
    //     std::cout << i << ' ';

    // The result is unspecified.
    std::cout <<
        (std::memcmp(v, original, sizeof(v)) == 0 ? "Unmodified\n" : "Modified\n");
}
```

Possible output:

```text
Default value
Default value
Default value
Unmodified
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3870 | C++20 | this algorithm might create objects on a const storage |
      kept disallowed

### See also

- **uninitialized_default_construct_n (C++17)** — constructs objects by
  default-initialization in an uninitialized area of memory, defined by a start
  and a count (function template)
- **uninitialized_value_construct (C++17)** — constructs objects by
  value-initialization in an uninitialized area of memory, defined by a range
  (function template)
- **ranges::uninitialized_default_construct (C++20)** — constructs objects by
  default-initialization in an uninitialized area of memory, defined by a range
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/memory/uninitialized_default_construct*
