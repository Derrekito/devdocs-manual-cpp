# std::basic_string<CharT,Traits,Allocator>::substr

```cpp
basic_string substr( size_type pos = 0, size_type count = npos ) const;  // (until C++20)
constexpr basic_string
    substr( size_type pos = 0, size_type count = npos ) const;  // (since C++20) (until C++23)
constexpr basic_string
    substr( size_type pos = 0, size_type count = npos ) const&;  // (since C++23)
constexpr basic_string substr( size_type pos = 0, size_type count = npos ) &&;  // (2) (since C++23)
```

Returns a substring `[``pos``,``pos + count``)`. If the requested substring
extends past the end of the string, i.e. the `count` is greater than `size() -
pos` (e.g. if `count == npos`), the returned substring is
`[``pos``,``size()``)`.

1) Equivalent to `return basic_string(*this, pos, count);`.

2) Equivalent to `return basic_string(std::move(*this), pos, count);`.

### Parameters

- **pos** — position of the first character to include
- **count** — length of the substring

### Return value

String containing the substring `[``pos``,``pos + count``)` or
`[``pos``,``size()``)`.

### Exceptions

`std::out_of_range` if `pos > size()`.

If an exception is thrown for any reason, these functions have no effect (strong
exception safety guarantee).

### Complexity

Linear in `count`.

### Notes

The allocator of the returned string is default-constructed: the new allocator
might *not* be a copy of `get_allocator()`.

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    std::string a = "0123456789abcdefghij";

    // count is npos, returns [pos, size())
    std::string sub1 = a.substr(10);
    std::cout << sub1 << '\n';

    // both pos and pos + count are within bounds, returns [pos, pos + count)
    std::string sub2 = a.substr(5, 3);
    std::cout << sub2 << '\n';

    // pos is within bounds, pos + count is not, returns [pos, size())
    std::string sub4 = a.substr(a.size() - 3, 50);
    // this is effectively equivalent to
    // std::string sub4 = a.substr(17, 3);
    // since a.size() == 20, pos == a.size() - 3 == 17, and a.size() - pos == 3

    std::cout << sub4 << '\n';

    try
    {
        // pos is out of bounds, throws
        std::string sub5 = a.substr(a.size() + 3, 50);
        std::cout << sub5 << '\n';
    }
    catch (const std::out_of_range& ex)
    {
        std::cout << ex.what() << '\n';
    }
}
```

Possible output:

```text
abcdefghij
567
hij
basic_string::substr: __pos (which is 23) > this->size() (which is 20)
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 847 | C++98 | there was no exception safety guarantee | added strong
      exception safety guarantee

### See also

- **copy** — copies characters (public member function)
- **sizelength** — returns the number of characters (public member function)
- **find** — finds the first occurrence of the given substring (public member
  function)
- **npos [static]** — special value. The exact meaning depends on the context
  (public static member constant)
- **substr** — returns a substring (public member function of
  `std::basic_string_view<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/substr*
