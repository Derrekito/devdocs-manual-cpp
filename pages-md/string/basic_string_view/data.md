# std::basic_string_view<CharT,Traits>::data

```cpp
constexpr const_pointer data() const noexcept;  // (since C++17)
```

Returns a pointer to the underlying character array. The pointer is such that
the range `[``data()``,``data() + size()``)` is valid and the values in it
correspond to the values of the view.

### Parameters

(none)

### Return value

A pointer to the underlying character array.

### Complexity

Constant.

### Notes

Unlike `std::basic_string::data()` and string literals,
`std::basic_string_view::data()` returns a pointer to a buffer that is not
necessarily null-terminated, for example a substring view (e.g. from
`remove_suffix`). Therefore, it is typically a mistake to pass `data()` to a
routine that takes just a `const CharT*` and expects a null-terminated string.

### Example

```cpp
#include <cstring>
#include <cwchar>
#include <iostream>
#include <string>
#include <string_view>

int main()
{
    std::wstring_view wcstr_v = L"xyzzy";
    std::cout << std::wcslen(wcstr_v.data()) << '\n';
    // OK: the underlying character array is null-terminated

    char array[3] = {'B', 'a', 'r'};
    std::string_view array_v(array, sizeof array);
    // std::cout << std::strlen(array_v.data()) << '\n';
    // error: the underlying character array is not null-terminated

    std::string str(array_v.data(), array_v.size()); // OK
    std::cout << std::strlen(str.data()) << '\n';
    // OK: the underlying character array of a std::string is always null-terminated
}
```

Output:

```text
5
3
```

### See also

- **front** — accesses the first character (public member function)
- **back** — accesses the last character (public member function)
- **data** — returns a pointer to the first character of a string (public member
  function of `std::basic_string<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/data*
