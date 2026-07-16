# std::basic_string<CharT,Traits,Allocator>::find

Finds the first occurrence of a substring, C string, character, or
`string_view`-like object, searching forward from `pos`. Returns the
starting index of the match, or **`std::string::npos`** if nothing
matches. `npos` is the largest possible `size_type` value — never
compare the result to `-1` (a signed `-1` and unsigned `npos` are not
even the same type, and the comparison can silently mislead).

```cpp skip
s.find(str);              // first occurrence of another string
s.find(str, pos);         // ...starting the search at pos
s.find(cstr);             // first occurrence of a C string
s.find(cstr, pos, count);  // first count chars of cstr, from pos
s.find(ch);                // first occurrence of a single character
s.find(ch, pos);
s.find(string_view_like);  // (since C++17) anything convertible to string_view
```

### What you provide

- **str / cstr / ch / t** — what to search for: another
  `basic_string`, a null-terminated `const CharT*`, a single `CharT`,
  or (since C++17) anything implicitly convertible to
  `std::basic_string_view` (e.g. `std::string_view`, another string).
- **pos** — index to start searching at (default `0`). If
  `pos >= size()`, a non-empty search always returns `npos`.
- **count** — (C-string overload only) treat only the first `count`
  characters of `s` as the pattern; that range may contain embedded
  null characters, unlike the null-terminated overload.

### Guarantees and costs

- Returns `npos` on no match — check with `== std::string::npos`
  (`!= npos` for "found"), not a numeric comparison.
- An empty pattern is considered found at `pos`, as long as
  `pos <= size()`.
- Overloads taking `str`, `ch`, or a `string_view`-like are
  `noexcept` (since C++11); the C-string overloads can throw only
  from `Traits::length`. `constexpr` since C++20.
- No standard complexity bound is given, but implementations are
  effectively linear in the searched range.

### Gotchas

- `find` searches forward from `pos`; use `rfind` to search backward
  from `pos` (still returns the *start* index of the match).
- The C-string overload `find(s, pos, count)` reads `count` characters
  from `s` regardless of embedded nulls — passing a mismatched
  `count` is undefined behavior, since `[s, s + count)` must be valid.
- `find` looks for a *substring*; to find any one of a set of
  characters, use `find_first_of` instead.

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    std::string s = "This is a string";

    std::string::size_type n = s.find("is");
    if (n != std::string::npos)
        std::cout << "found at " << n << '\n';

    n = s.find("is", 5);         // start searching past the first hit
    std::cout << "found at " << n << '\n';

    n = s.find('q');             // not present
    if (n == std::string::npos)
        std::cout << "not found\n";
}
```

```text
found at 2
found at 5
not found
```

### Reference

```cpp skip
size_type find( const basic_string& str, size_type pos = 0 ) const;  // (until C++11)
size_type find( const basic_string& str, size_type pos = 0 ) const noexcept;  // (since C++11) (until C++20)
constexpr size_type find( const basic_string& str,
                          size_type pos = 0 ) const noexcept;  // (since C++20)
size_type find( const CharT* s, size_type pos, size_type count ) const;  // (until C++20)
constexpr size_type find( const CharT* s,
                          size_type pos, size_type count ) const;  // (since C++20)
size_type find( const CharT* s, size_type pos = 0 ) const;  // (until C++20)
constexpr size_type find( const CharT* s, size_type pos = 0 ) const;  // (since C++20)
size_type find( CharT ch, size_type pos = 0 ) const;  // (until C++11)
size_type find( CharT ch, size_type pos = 0 ) const noexcept;  // (since C++11) (until C++20)
constexpr size_type find( CharT ch, size_type pos = 0 ) const noexcept;  // (since C++20)
template< class StringViewLike >
size_type find( const StringViewLike& t,
                size_type pos = 0 ) const noexcept(/* see below */);  // (since C++17) (until C++20)
template< class StringViewLike >
constexpr size_type find( const StringViewLike& t,
                          size_type pos = 0 ) const noexcept(/* see below */);  // (since C++20)
```

Formally, `str` is found at `xpos` iff `xpos >= pos`,
`xpos + str.size() <= size()`, and every character in `str` matches
via `Traits::eq`. The `StringViewLike` overload participates in
overload resolution only if `t` converts to
`std::basic_string_view<CharT, Traits>` but not to `const CharT*`.

### See also

- **rfind** — find the last occurrence of a substring
- **find_first_of** — find the first occurrence of any character in a set
- **find_first_not_of** — find the first character not in a set
- **find_last_of** — find the last occurrence of any character in a set
- **find_last_not_of** — find the last character not in a set
- **find** — the non-owning `std::basic_string_view` equivalent
- **search** — general substring search over arbitrary ranges

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/find*
