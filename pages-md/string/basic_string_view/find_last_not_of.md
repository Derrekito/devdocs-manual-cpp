# std::basic_string_view<CharT,Traits>::find_last_not_of

```cpp
constexpr size_type
    find_last_not_of( basic_string_view v, size_type pos = npos ) const noexcept;  // (1) (since C++17)
constexpr size_type
    find_last_not_of( CharT ch, size_type pos = npos ) const noexcept;  // (2) (since C++17)
constexpr size_type
    find_last_not_of( const CharT* s, size_type pos, size_type count ) const;  // (3) (since C++17)
constexpr size_type
    find_last_not_of( const CharT* s, size_type pos = npos ) const;  // (4) (since C++17)
```

Finds the last character not equal to any of the characters in the given
character sequence. The search considers only the interval `[``​0​``,``pos``]`.

1) Finds the last character not equal to any of the characters of `v` in this
   view, starting at position `pos`.

2) Equivalent to `find_last_not_of(basic_string_view(std::addressof(ch), 1),
   pos)`.

3) Equivalent to `find_last_not_of(basic_string_view(s, count), pos)`.

4) Equivalent to `find_last_not_of(basic_string_view(s), pos)`.

### Parameters

- **v** — view to search for
- **pos** — position at which to start the search
- **count** — length of the string of characters to compare
- **s** — pointer to a string of characters to compare
- **ch** — character to compare

### Return value

Position of the last character not equal to any of the characters in the given
string, or `npos` if no such character is found.

### Complexity

O(`size()``* v.``size()`) at worst.

### Example

```cpp
#include <string_view>
using std::operator""sv;

int main()
{
    static_assert(1 == "BCDEF"sv.find_last_not_of("DEF"));
                    //   ^
    static_assert(2 == "BCDEFG"sv.find_last_not_of("EFG", 3));
                    //    ^
    static_assert(2 == "ABBA"sv.find_last_not_of('A'));
                    //    ^
    static_assert(1 == "ABBA"sv.find_last_not_of('A', 1));
                    //   ^
}
```

### See also

- **find** — find characters in the view (public member function)
- **rfind** — find the last occurrence of a substring (public member function)
- **find_first_of** — find first occurrence of characters (public member
  function)
- **find_last_of** — find last occurrence of characters (public member function)
- **find_first_not_of** — find first absence of characters (public member
  function)
- **find_last_not_of** — find last absence of characters (public member function
  of `std::basic_string<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/find_last_not_of*
