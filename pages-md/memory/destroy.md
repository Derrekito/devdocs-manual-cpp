# std::destroy

```cpp
template< class ForwardIt >
void destroy( ForwardIt first, ForwardIt last );  // (since C++17) (until C++20)
template< class ForwardIt >
constexpr void destroy( ForwardIt first, ForwardIt last );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt >
void destroy( ExecutionPolicy&& policy, ForwardIt first, ForwardIt last );  // (2) (since C++17)
```

1) Destroys the objects in the range `[``first``,``last``)`, as if by for (;
   first != last; ++first) std::destroy_at(std::addressof(*first));

2) Same as (1), but executed according to `policy`. This overload does not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first, last** — the range of elements to destroy
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
constexpr // since C++20
void destroy(ForwardIt first, ForwardIt last)
{
    for (; first != last; ++first)
        std::destroy_at(std::addressof(*first));
}
```

### Example

The following example demonstrates how to use `destroy` to destroy a contiguous
sequence of elements.

```cpp
#include <iostream>
#include <memory>
#include <new>

struct Tracer
{
    int value;
    ~Tracer() { std::cout << value << " destructed\n"; }
};

int main()
{
    alignas(Tracer) unsigned char buffer[sizeof(Tracer) * 8];

    for (int i = 0; i < 8; ++i)
        new(buffer + sizeof(Tracer) * i) Tracer{i}; //manually construct objects

    auto ptr = std::launder(reinterpret_cast<Tracer*>(buffer));

    std::destroy(ptr, ptr + 8);
}
```

Output:

```text
0 destructed
1 destructed
2 destructed
3 destructed
4 destructed
5 destructed
6 destructed
7 destructed
```

### See also

- **destroy_n (C++17)** — destroys a number of objects in a range (function
  template)
- **destroy_at (C++17)** — destroys an object at a given address (function
  template)
- **ranges::destroy (C++20)** — destroys a range of objects (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/memory/destroy*
