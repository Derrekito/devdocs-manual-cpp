# std::basic_string_view<CharT,Traits>::begin, std::basic_string_view<CharT,Traits>::cbegin

```cpp
constexpr const_iterator begin() const noexcept;  // (since C++17)
constexpr const_iterator cbegin() const noexcept;  // (since C++17)
```

Returns an iterator to the first character of the view.

### Parameters

(none)

### Return value

`const_iterator` to the first character.

### Complexity

Constant.

### Example

```cpp
#include <concepts>
#include <string_view>

int main()
{
    constexpr std::string_view str_view("abcd");

    constexpr auto begin = str_view.begin();
    constexpr auto cbegin = str_view.cbegin();
    static_assert(
        *begin == 'a' and
        *cbegin == 'a' and
        *begin == *cbegin and
        begin == cbegin and
        std::same_as<decltype(begin), decltype(cbegin)>);
}
```

### See also

- **endcend** — returns an iterator to the end (public member function)
- **begincbegin (DR*)** — returns an iterator to the beginning (public member
  function of `std::basic_string<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/begin*
