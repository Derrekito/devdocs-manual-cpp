# std::basic_string_view<CharT,Traits>::rend, std::basic_string_view<CharT,Traits>::crend

```cpp
constexpr const_reverse_iterator rend() const noexcept;  // (since C++17)
constexpr const_reverse_iterator crend() const noexcept;  // (since C++17)
```

Returns a reverse iterator to the character following the last character of the
reversed view. It corresponds to the character preceding the first character of
the non-reversed view. This character acts as a placeholder, attempting to
access it results in undefined behavior.

### Parameters

(none)

### Return value

`const_reverse_iterator` to the character following the last character.

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

    std::copy(str_view.rbegin(), str_view.rend(), out_it);
    *out_it = '\n';

    std::copy(str_view.crbegin(), str_view.crend(), out_it);
    *out_it = '\n';
}
```

Output:

```text
fedcba
fedcba
```

### See also

- **rbegincrbegin** — returns a reverse iterator to the beginning (public member
  function)
- **rendcrend (DR*)** — returns a reverse iterator to the end (public member
  function of `std::basic_string<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/rend*
