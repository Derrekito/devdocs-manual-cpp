# std::basic_string_view<CharT,Traits>::rbegin, std::basic_string_view<CharT,Traits>::crbegin

```cpp
constexpr const_reverse_iterator rbegin() const noexcept;  // (since C++17)
constexpr const_reverse_iterator crbegin() const noexcept;  // (since C++17)
```

Returns a reverse iterator to the first character of the reversed view. It
corresponds to the last character of the non-reversed view.

### Parameters

(none)

### Return value

`const_reverse_iterator` to the first character.

### Complexity

Constant.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <string_view>

int main()
{
    std::ostream_iterator<char> out_it(std::cout);
    std::string_view str_view("abcdef");

    std::copy(str_view.rbegin(), std::next(str_view.rbegin(), 3), out_it);
    *out_it = '\n';

    std::copy(str_view.crbegin(), std::next(str_view.crbegin(), 3), out_it);
    *out_it = '\n';
}
```

Output:

```text
fed
fed
```

### See also

- **rendcrend** — returns a reverse iterator to the end (public member function)
- **rbegincrbegin (DR*)** — returns a reverse iterator to the beginning (public
  member function of `std::basic_string<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/rbegin*
