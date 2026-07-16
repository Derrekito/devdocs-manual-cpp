# std::basic_string_view<CharT,Traits>::find_first_of

```cpp
constexpr size_type find_first_of( basic_string_view v, size_type pos = 0 ) const noexcept;  // (1) (since C++17)
constexpr size_type find_first_of( CharT ch, size_type pos = 0 ) const noexcept;  // (2) (since C++17)
constexpr size_type find_first_of( const CharT* s, size_type pos, size_type count ) const;  // (3) (since C++17)
constexpr size_type find_first_of( const CharT* s, size_type pos = 0 ) const;  // (4) (since C++17)
```

Finds the first character equal to any of the characters in the given character
sequence.

1) Finds the first occurrence of any of the characters of `v` in this view,
   starting at position `pos`.

2) Equivalent to `find_first_of(basic_string_view(std::addressof(ch), 1), pos)`.

3) Equivalent to `find_first_of(basic_string_view(s, count), pos)`.

4) Equivalent to `find_first_of(basic_string_view(s), pos)`.

### Parameters

- **v** — view to search for
- **pos** — position at which to start the search
- **count** — length of the string of characters to search for
- **s** — pointer to a string of characters to search for
- **ch** — character to search for

### Return value

Position of the first occurrence of any character of the substring, or `npos` if
no such character is found.

### Complexity

O(`size()` * v.`size()`) at worst.

### Example

```cpp
#include <iostream>
#include <string_view>

int main()
{
    using namespace std::literals;
    constexpr auto N = std::string_view::npos;

    auto is_white_space = [](const char c) noexcept
    {
        return " \t\n\f\r\v"sv.find_first_of(c) != N;
    };

    static_assert(
        1 == "alignas"sv.find_first_of("klmn"sv) &&
          //   └─────────────────────────┘
        N == "alignof"sv.find_first_of("wxyz"sv) &&
          //
        3 == "concept"sv.find_first_of("bcde"sv, /* pos= */ 1) &&
          //     └───────────────────────┘
        N == "consteval"sv.find_first_of("oxyz"sv, /* pos= */ 2) &&
          //
        6 == "constexpr"sv.find_first_of('x') &&
          //        └─────────────────────┘
        N == "constinit"sv.find_first_of('x') &&
          //
        6 == "const_cast"sv.find_first_of('c', /* pos= */ 4) &&
          //        └──────────────────────┘
        N == "continue"sv.find_first_of('c', /* pos= */ 42) &&
          //
        5 == "co_await"sv.find_first_of("cba", /* pos= */ 4) &&
          //       └───────────────────────┘
        7 == "decltype"sv.find_first_of("def", /* pos= */ 2, /* count= */ 2) &&
          //         └────────────────────┘
        N == "decltype"sv.find_first_of("def", /* pos= */ 2, /* count= */ 1) &&
          //
        is_white_space(' ') && is_white_space('\r') && !is_white_space('\a')
    );
    std::cout << "All tests passed.\n";
}
```

Output:

```text
All tests passed.
```

### See also

- **find** — find characters in the view (public member function)
- **rfind** — find the last occurrence of a substring (public member function)
- **find_last_of** — find last occurrence of characters (public member function)
- **find_first_not_of** — find first absence of characters (public member
  function)
- **find_last_not_of** — find last absence of characters (public member
  function)
- **find_first_of** — find first occurrence of characters (public member
  function of `std::basic_string<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/find_first_of*
