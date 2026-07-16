# std::basic_string_view<CharT,Traits>::rfind

```cpp
constexpr size_type rfind( basic_string_view v, size_type pos = npos ) const noexcept;  // (1) (since C++17)
constexpr size_type rfind( CharT ch, size_type pos = npos ) const noexcept;  // (2) (since C++17)
constexpr size_type rfind( const CharT* s, size_type pos, size_type count ) const;  // (3) (since C++17)
constexpr size_type rfind( const CharT* s, size_type pos = npos ) const;  // (4) (since C++17)
```

Finds the last substring that is equal to the given character sequence. The
search begins at `pos` and proceeds from right to left (thus, the found
substring, if any, cannot begin in a position following `pos`). If `npos` or any
value not smaller than `size() - 1` is passed as `pos`, the whole string will be
searched.

1) Finds the last occurrence of `v` in this view, starting at position `pos`.

2) Equivalent to `rfind(basic_string_view(std::addressof(ch), 1), pos)`.

3) Equivalent to `rfind(basic_string_view(s, count), pos)`.

4) Equivalent to `rfind(basic_string_view(s), pos)`.

### Parameters

- **v** — view to search for
- **pos** — position at which to start the search
- **count** — length of substring to search for
- **s** — pointer to a character string to search for
- **ch** — character to search for

### Return value

Position of the first character of the found substring or `npos` if no such
substring is found.

### Complexity

O(`size()` * v.`size()`) at worst.

### Example

```cpp
#include <string_view>

int main()
{
    using namespace std::literals;
    constexpr auto N = std::string_view::npos;

    static_assert(true
        && (6 == "AB AB AB"sv.rfind("AB"))
        && (6 == "AB AB AB"sv.rfind("ABCD", N, 2))
        && (3 == "AB AB AB"sv.rfind("AB", 5))
        && (0 == "AB CD EF"sv.rfind("AB", 0))
        && (2 == "B AB AB "sv.rfind("AB", 2))
        && (N == "B AB AB "sv.rfind("AB", 1))
        && (5 == "B AB AB "sv.rfind('A'))
        && (4 == "AB AB AB"sv.rfind('B', 4))
        && (N == "AB AB AB"sv.rfind('C'))
    );
}
```

### See also

- **find** — find characters in the view (public member function)
- **find_first_of** — find first occurrence of characters (public member
  function)
- **find_last_of** — find last occurrence of characters (public member function)
- **find_first_not_of** — find first absence of characters (public member
  function)
- **find_last_not_of** — find last absence of characters (public member
  function)
- **rfind** — find the last occurrence of a substring (public member function of
  `std::basic_string<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/rfind*
