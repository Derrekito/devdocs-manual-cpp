# std::basic_string_view<CharT,Traits>::end, std::basic_string_view<CharT,Traits>::cend

```cpp
constexpr const_iterator end() const noexcept;  // (since C++17)
constexpr const_iterator cend() const noexcept;  // (since C++17)
```

Returns an iterator to the character following the last character of the view.
This character acts as a placeholder, attempting to access it results in
undefined behavior.

### Parameters

(none)

### Return value

`const_iterator` to the character following the last character.

### Complexity

Constant.

### Example

```cpp
#include <iostream>
#include <iterator>
#include <string_view>

int main()
{
    constexpr std::string_view str_view("abcd");
    constexpr auto end = str_view.end();
    constexpr auto cend = str_view.cend();

    static_assert
    (
        *std::prev(end) == 'd' && 'd' == *std::prev(cend) and end == cend
    );
}
```

### See also

- **begincbegin** — returns an iterator to the beginning (public member
  function)
- **endcend (DR*)** — returns an iterator to the end (public member function of
  `std::basic_string<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/end*
