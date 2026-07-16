# std::basic_string<CharT,Traits,Allocator>::replace_with_range

```cpp
template< container-compatible-range<CharT> R >
constexpr std::basic_string& replace_with_range( const_iterator first,
                                                 const_iterator last,
                                                 R&& rg );  // (since C++23)
```

Replaces the characters in the range `[``first``,``last``)` with the characters
from the range `rg`.

Equivalent to

```cpp
return replace(first,
               last,
               std::basic_string(
                   std::from_range,
                   std::forward<R>(rg),
                   get_allocator())
);
```

### Parameters

- **first, last** — range of characters that is going to be replaced
- **rg** — a container compatible range

### Return value

`*this`

### Complexity

Linear in size of `rg`.

### Exceptions

If the operation would result in `size() > max_size()`, throws
`std::length_error`.

If an exception is thrown for any reason, this function has no effect (strong
exception safety guarantee).

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_containers_ranges` | 202202L | (C++23) | member functions that
      accept container compatible range

### Example

```cpp
#include <algorithm>
#include <cassert>
#include <forward_list>
#include <iterator>
#include <string>

int main()
{
    using namespace std::literals;

    auto s{"Today is today!"s};
    constexpr auto today{"today"sv};
    constexpr auto tomorrow{"tomorrow's yesterday"sv};
    std::forward_list<char> rg;
    std::ranges::reverse_copy(tomorrow, std::front_inserter(rg));

    const auto pos{s.rfind(today)};
    assert(pos != s.npos);
    const auto first{std::next(s.begin(), pos)};
    const auto last{std::next(first, today.length())};

#ifdef __cpp_lib_containers_ranges
    s.replace_range(first, last, rg);
#else
    s.replace(first, last, rg.cbegin(), rg.cend());
#endif

    assert("Today is tomorrow's yesterday!" == s);
}
```

### See also

- **replace** — replaces specified portion of a string (public member function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/replace_with_range*
