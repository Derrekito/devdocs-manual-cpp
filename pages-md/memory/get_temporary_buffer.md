# std::get_temporary_buffer

```cpp
template< class T >
std::pair<T*, std::ptrdiff_t>
    get_temporary_buffer( std::ptrdiff_t count );  // (until C++11)
template< class T >
std::pair<T*, std::ptrdiff_t>
    get_temporary_buffer( std::ptrdiff_t count ) noexcept;  // (since C++11) (deprecated in C++17) (removed in C++20)
```

If `count` is negative or zero, does nothing.

Otherwise, requests to allocate uninitialized contiguous storage for `count`
adjacent objects of type `T`. The request is non-binding, and the implementation
may instead allocate the storage for any other number of (including zero)
adjacent objects of type `T`.

It is implementation-defined whether over-aligned types are supported.
*(since C++11)*

### Parameters

- **count** — the desired number of objects

### Return value

A `std::pair`, the member `first` is a pointer to the beginning of the allocated
storage and the member `second` is the number of objects that fit in the storage
that was actually allocated.

If `count <= 0` or allocated storage is not enough to store a single element of
type `T`, the member `first` of the result is a null pointer and the member
`second` is zero.

### Notes

This API was originally designed with the intent of providing a more efficient
implementation than the general-purpose operator new, but no such implementation
was created and the API was deprecated and removed.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <memory>
#include <string>

int main()
{
    const std::string s[] = {"string", "1", "test", "..."};
    const auto p = std::get_temporary_buffer<std::string>(4);
    // requires that p.first is passed to return_temporary_buffer
    // (beware of early exit points and exceptions), or better use:
    std::unique_ptr<std::string, void(*)(std::string*)> on_exit(p.first,
    [](std::string* p)
    {
        std::cout << "returning temporary buffer...\n";
        std::return_temporary_buffer(p);
    });

    std::copy(s, s + p.second,
              std::raw_storage_iterator<std::string*, std::string>(p.first));
    // has same effect as: std::uninitialized_copy(s, s + p.second, p.first);
    // requires that each string in p is individually destroyed
    // (beware of early exit points and exceptions)

    std::copy(p.first, p.first + p.second,
              std::ostream_iterator<std::string>{std::cout, "\n"});

    std::for_each(p.first, p.first + p.second, [](std::string& e)
    {
        e.~basic_string<char>();
    }); // same as: std::destroy(p.first, p.first + p.second);

    // manually reclaim memory if unique_ptr-like technique is not used:
    // std::return_temporary_buffer(p.first);
}
```

Output:

```text
string
1
test
...
returning temporary buffer...
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 425 | C++98 | the behavior when `count <= 0` was unclear | made clear
  LWG 2072 | C++98 | it was not allowed to allocate insufficient memory |
      allowed

### See also

- **return_temporary_buffer (deprecated in C++17)(removed in C++20)** — frees
  uninitialized storage (function template)
- **allocate_at_least [static] (C++23)** — allocates storage at least as large
  as the requested size via an allocator (public static member function of
  `std::allocator_traits<Alloc>`)

---
*Source: https://en.cppreference.com/w/cpp/memory/get_temporary_buffer*
