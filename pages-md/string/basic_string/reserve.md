# std::basic_string<CharT,Traits,Allocator>::reserve

```cpp
void reserve( size_type new_cap = 0 );  // (until C++20)
constexpr void reserve( size_type new_cap );  // (since C++20)
void reserve();  // (2) (deprecated in C++20) (removed in C++26)
```

1) Informs a `std::basic_string` object of a planned change in size, so that it
can manage the storage allocation appropriately.

- If `new_cap` is greater than the current `capacity()`, new storage is
  allocated, and `capacity()` is made equal or greater than `new_cap`.

- If `new_cap` is less than the current `capacity()`, this is a non-binding
  shrink request.
- If `new_cap` is less than the current `size()`, this is a non-binding
  shrink-to-fit request equivalent to `shrink_to_fit()`(since C++11).
*(until C++20)*

- If `new_cap` is less than or equal to the current `capacity()`, there is no
  effect.
*(since C++20)*

If a capacity change takes place, all iterators and references, including the
   past-the-end iterator, are invalidated.

2) A non-binding shrink-to-fit request. After this call, `capacity()` has an
   unspecified value greater than or equal to `size()`.

### Parameters

- **new_cap** — new capacity of the string

### Return value

(none)

### Exceptions

Throws `std::length_error` if `new_cap` is greater than `max_size()`.

May throw any exceptions thrown by
`std::allocator_traits<Allocator>::allocate()`, such as `std::bad_alloc`.

If an exception is thrown for any reason, this function has no effect (strong
exception safety guarantee).

### Complexity

At most linear in the `size()` of the string.

### Example

```cpp
#include <cassert>
#include <iostream>
#include <string>

int main()
{
    std::string s;
    std::cout << "1) Initially: " << s.capacity() << '\n';

    const std::string::size_type new_cap{101u};
    s.reserve(new_cap);
    assert(s.capacity() >= new_cap);
    std::cout << "2) After reserve(" << new_cap << "): " << s.capacity() << '\n';

    // observing the capacity growth factor
    auto cap{s.capacity()};
    for (int check{}; check != 4; ++check)
    {
        while (cap == s.capacity())
            s += '$';
        cap = s.capacity();
        std::cout << (3) + check << ") Capacity: " << cap << '\n';
    }

//  s.reserve(); // deprecated/removed in C++20/26, use:
    s.shrink_to_fit();
    std::cout << "7) After shrink_to_fit: " << s.capacity() << '\n';
}
```

Possible output:

```text
1) Initially: 15
2) After reserve(101): 101
3) Capacity: 202
4) Capacity: 404
5) Capacity: 808
6) Capacity: 1616
7) After shrink_to_fit: 809
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 847 | C++98 | there was no exception safety guarantee | added strong
      exception safety guarantee

### See also

- **capacity** — returns the number of characters that can be held in currently
  allocated storage (public member function)
- **resize** — changes the number of characters stored (public member function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/reserve*
