# std::basic_string_view<CharT,Traits>::find

```cpp
constexpr size_type find( basic_string_view v, size_type pos = 0 ) const noexcept;  // (1) (since C++17)
constexpr size_type find( CharT ch, size_type pos = 0 ) const noexcept;  // (2) (since C++17)
constexpr size_type find( const CharT* s, size_type pos, size_type count ) const;  // (3) (since C++17)
constexpr size_type find( const CharT* s, size_type pos = 0 ) const;  // (4) (since C++17)
```

Finds the first substring equal to the given character sequence.

1) Finds the first occurence of `v` in this view, starting at position `pos`.

2) Equivalent to `find(basic_string_view(std::addressof(ch), 1), pos)`.

3) Equivalent to `find(basic_string_view(s, count), pos)`.

4) Equivalent to `find(basic_string_view(s), pos)`.

### Parameters

- **v** — view to search for
- **pos** — position at which to start the search
- **count** — length of substring to search for
- **s** — pointer to a character string to search for
- **ch** — character to search for

### Return value

Position of the first character of the found substring, or `npos` if no such
substring is found.

### Complexity

O(`size()` * v.`size()`) at worst.

### Example

```cpp
#include <string_view>

int main()
{
    using namespace std::literals;

    constexpr auto str{" long long int;"sv};

    static_assert(
        1 == str.find("long"sv)            && "<- find(v , pos = 0)" &&
        6 == str.find("long"sv, 2)         && "<- find(v , pos = 2)" &&
        0 == str.find(' ')                 && "<- find(ch, pos = 0)" &&
        2 == str.find('o', 1)              && "<- find(ch, pos = 1)" &&
        2 == str.find("on")                && "<- find(s , pos = 0)" &&
        6 == str.find("long double", 5, 4) && "<- find(s , pos = 5, count = 4)"
    );

    static_assert(str.npos == str.find("float"));
}
```

### See also

- **rfind** — find the last occurrence of a substring (public member function)
- **find_first_of** — find first occurrence of characters (public member
  function)
- **find_last_of** — find last occurrence of characters (public member function)
- **find_first_not_of** — find first absence of characters (public member
  function)
- **find_last_not_of** — find last absence of characters (public member
  function)
- **find** — finds the first occurrence of the given substring (public member
  function of `std::basic_string<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/find*
