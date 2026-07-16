# std::basic_string<CharT,Traits,Allocator>::size, std::basic_string<CharT,Traits,Allocator>::length

```cpp
size_type size() const;  // (until C++11)
size_type size() const noexcept;  // (since C++11) (until C++20)
constexpr size_type size() const noexcept;  // (since C++20)
size_type length() const;  // (until C++11)
size_type length() const noexcept;  // (since C++11) (until C++20)
constexpr size_type length() const noexcept;  // (since C++20)
```

Returns the number of `CharT` elements in the string, i.e.
`std::distance(begin(), end())`.

### Parameters

(none)

### Return value

The number of `CharT` elements in the string.

### Complexity

Unspecified
*(until C++11)*

Constant
*(since C++11)*

### Notes

For `std::string`, the elements are bytes (objects of type char), which are not
the same as characters if a multibyte encoding such as UTF-8 is used.

### Example

```cpp
#include <cassert>
#include <iterator>
#include <string>

int main()
{
    std::string s("Exemplar");
    assert(8 == s.size());
    assert(s.size() == s.length());
    assert(s.size() == static_cast<std::string::size_type>(
        std::distance(s.begin(), s.end())));

    std::u32string a(U"ハロー・ワールド"); // 8 code points
    assert(8 == a.size()); // 8 code units in UTF-32

    std::u16string b(u"ハロー・ワールド"); // 8 code points
    assert(8 == b.size()); // 8 code units in UTF-16

    std::string c("ハロー・ワールド"); // 8 code points
    assert(24 == c.size()); // 24 code units in UTF-8

    #if __cplusplus >= 202002
    std::u8string d(u8"ハロー・ワールド"); // 8 code points
    assert(24 == d.size()); // 24 code units in UTF-8
    #endif
}
```

### See also

- **empty** — checks whether the string is empty (public member function)
- **max_size** — returns the maximum number of characters (public member
  function)
- **sizelength** — returns the number of characters (public member function of
  `std::basic_string_view<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/size*
