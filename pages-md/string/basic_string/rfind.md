# std::basic_string<CharT,Traits,Allocator>::rfind

`rfind` is `find` run from the right: it looks for the *last*
occurrence of a substring, C string, character, or `string_view`-like
object, scanning right to left. It still returns the *starting*
index of the match, not the end. `pos` means "the match may not begin
after here" — the search effectively starts at `min(pos, size())` and
walks backward, so `pos = npos` (the default) searches the whole
string, and any `pos >= size() - 1` behaves the same as no `pos` at
all.

```cpp skip
s.rfind(str);              // last occurrence of another string
s.rfind(str, pos);         // ...match must not start after pos
s.rfind(cstr);             // last occurrence of a C string
s.rfind(cstr, pos, count);  // last occurrence of the first count chars of cstr
s.rfind(ch);                // last occurrence of a single character
s.rfind(ch, pos);
s.rfind(string_view_like);  // (since C++17) anything convertible to string_view
```

### What you provide

- **str / cstr / ch / t** — what to search for: another
  `basic_string`, a null-terminated `const CharT*`, a single `CharT`,
  or (since C++17) anything implicitly convertible to
  `std::basic_string_view`.
- **pos** — the rightmost start position to consider (default `npos`,
  meaning "no limit — search the whole string"). The match's start
  index must be `<= pos`.
- **count** — (C-string overload only) treat only the first `count`
  characters of `s` as the pattern.

### Guarantees and costs

- Returns `npos` if no match satisfies `xpos <= pos`.
- An empty pattern is always "found": at `pos` if `pos < size()`,
  otherwise at `size()` (this covers `pos == npos` too).
- If the string itself is empty (and the pattern isn't), `npos` is
  always returned.
- Overloads taking `str`, `ch`, or a `string_view`-like are `noexcept`
  (since C++11); the C-string overloads can throw only from
  `Traits::length`. `constexpr` since C++20.
- Strong exception safety: if it throws, the string is unchanged.

### Gotchas

- `pos` bounds where the match may *start*, not where scanning
  begins from the right — `s.rfind("is", 4)` still requires the match
  to start at index `<= 4`, even though characters past index 4 exist.
- Like `find`, the return value is the match's start index, so
  `s.rfind("is", 0)` is a common idiom for "does `s` start with
  `\"is\"`" (equivalent to, and predates, `starts_with`).
- Don't confuse `rfind` with `find_last_of`: `rfind` matches a whole
  substring; `find_last_of` matches any one character from a set.

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    std::string s = "This is a string";

    std::string::size_type n = s.rfind("is");        // search whole string
    std::cout << n << '\n';

    n = s.rfind("is", 4);                              // match must start <= 4
    std::cout << n << '\n';

    n = s.rfind("This", 0);                             // prefix check idiom
    std::cout << (n == 0 ? "starts with This" : "no") << '\n';

    n = s.rfind('q');                                   // not present
    std::cout << (n == std::string::npos ? "not found" : "found") << '\n';
}
```

```text
5
2
starts with This
not found
```

### Reference

```cpp skip
size_type rfind( const basic_string& str, size_type pos = npos ) const;  // (until C++11)
size_type rfind( const basic_string& str,
                 size_type pos = npos ) const noexcept;  // (since C++11) (until C++20)
constexpr size_type rfind( const basic_string& str,
                           size_type pos = npos ) const noexcept;  // (since C++20)
size_type rfind( const CharT* s, size_type pos, size_type count ) const;  // (until C++20)
constexpr size_type rfind( const CharT* s,
                           size_type pos, size_type count ) const;  // (since C++20)
size_type rfind( const CharT* s, size_type pos = npos ) const;  // (until C++20)
constexpr size_type rfind( const CharT* s, size_type pos = npos ) const;  // (since C++20)
size_type rfind( CharT ch, size_type pos = npos ) const;  // (until C++11)
size_type rfind( CharT ch, size_type pos = npos ) const noexcept;  // (since C++11) (until C++20)
constexpr size_type rfind( CharT ch, size_type pos = npos ) const noexcept;  // (since C++20)
template< class StringViewLike >
size_type rfind( const StringViewLike& t,
                 size_type pos = npos ) const noexcept(/* see below */);  // (since C++17) (until C++20)
template< class StringViewLike >
constexpr size_type
    rfind( const StringViewLike& t,
           size_type pos = npos ) const noexcept(/* see below */);  // (since C++20)
```

Equality is checked via `Traits::eq`. The `StringViewLike` overload
participates in overload resolution only if `t` converts to
`std::basic_string_view<CharT, Traits>` but not to `const CharT*`.
LWG 847 (C++98) added the strong exception safety guarantee
retroactively; LWG 2064 removed erroneous `noexcept` from the
C-string/char overloads in C++11, later restored by P1148R0.

### See also

- **find** — find the first occurrence of a substring
- **find_first_of** — find the first occurrence of any character in a set
- **find_last_of** — find the last occurrence of any character in a set
- **starts_with** — checks if the string begins with a prefix (C++20)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/rfind*
