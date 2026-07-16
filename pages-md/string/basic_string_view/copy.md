# std::basic_string_view<CharT,Traits>::copy

```cpp
size_type copy( CharT* dest, size_type count, size_type pos = 0 ) const;  // (since C++17) (until C++20)
constexpr size_type copy( CharT* dest, size_type count,
                          size_type pos = 0 ) const;  // (since C++20)
```

Copies the substring `[``pos``,``pos + rcount``)` to the character array pointed
to by `dest`, where `rcount` is the smaller of `count` and `size() - pos`.

Equivalent to `Traits::copy(dest, data() + pos, rcount)`.

### Parameters

- **dest** — pointer to the destination character string
- **count** — requested substring length
- **pos** — position of the first character

### Return value

Number of characters copied.

### Exceptions

`std::out_of_range` if `pos > size()`.

### Complexity

Linear in `rcount`.

### Example

```cpp
#include <array>
#include <cstddef>
#include <iostream>
#include <stdexcept>
#include <string_view>

int main()
{
    constexpr std::basic_string_view<char> source{"ABCDEF"};
    std::array<char, 8> dest;
    std::size_t count{}, pos{};

    dest.fill('\0');
    source.copy(dest.data(), count = 4); // pos = 0
    std::cout << dest.data() << '\n'; // ABCD

    dest.fill('\0');
    source.copy(dest.data(), count = 4, pos = 1);
    std::cout << dest.data() << '\n'; // BCDE

    dest.fill('\0');
    source.copy(dest.data(), count = 42, pos = 2); // ok, count -> 4
    std::cout << dest.data() << '\n'; // CDEF

    try
    {
        source.copy(dest.data(), count = 1, pos = 666); // throws: pos > size()
    }
    catch (std::out_of_range const& ex)
    {
        std::cout << ex.what() << '\n';
    }
}
```

Output:

```text
ABCD
BCDE
CDEF
basic_string_view::copy: __pos (which is 666) > __size (which is 6)
```

### See also

- **substr** — returns a substring (public member function)
- **copy** — copies characters (public member function of
  `std::basic_string<CharT,Traits,Allocator>`)
- **copycopy_if (C++11)** — copies a range of elements to a new location
  (function template)
- **memcpy** — copies one buffer to another (function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/copy*
