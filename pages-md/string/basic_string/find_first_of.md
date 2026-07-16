# std::basic_string<CharT,Traits,Allocator>::find_first_of

The #1 confusion with `find`: `find_first_of` does **not** search for
a substring. It searches for the first character that matches *any
one* character in the given set, treating the argument as a bag of
candidate characters, not a pattern to match in sequence. Searches
forward from `pos`; returns `npos` if none of the set's characters
appear.

```cpp skip
s.find_first_of(str);               // first char that's in str, from pos 0
s.find_first_of(str, pos);          // ...starting the search at pos
s.find_first_of(cstr);              // first char that's in cstr
s.find_first_of(cstr, pos, count);  // ...only cstr[0..count) is the set
s.find_first_of(ch);                // same as find(ch): one character
s.find_first_of(ch, pos);
s.find_first_of(string_view_like);  // (since C++17) convertible to string_view
```

### What you provide

- **str / cstr / ch / t** — the *set* of characters to match against:
  another `basic_string`, a null-terminated `const CharT*`, a single
  `CharT`, or (since C++17) anything implicitly convertible to
  `std::basic_string_view`. Order within the set doesn't matter.
- **pos** — index to start searching at (default `0`).
- **count** — (C-string overload only) treat only the first `count`
  characters of `s` as the set; may contain embedded nulls.

### Guarantees and costs

- Returns `npos` if no character in `[pos, size())` matches any
  character of the set.
- Overloads taking `str`, `ch`, or a `string_view`-like are `noexcept`
  (since C++11); the C-string overloads can throw only from
  `Traits::length`. `constexpr` since C++20.
- Strong exception safety: if it throws, the string is unchanged.
- `Traits::eq` performs each character comparison.

### Gotchas

- `s.find_first_of("abc")` finds the first character that is `'a'`,
  `'b'`, *or* `'c'` — not the substring `"abc"`. Use `find` for that.
- With a single `ch` argument, `find_first_of` and `find` behave
  identically (a one-character set is the same as one character), so
  the distinction only bites with multi-character arguments.
- The C-string `(s, pos, count)` overload reads exactly `count`
  characters from `s`; a mismatched `count` is undefined behavior.

### Example

```cpp
#include <cassert>
#include <string>

int main()
{
    using namespace std::literals;

    // "alignas" contains 'l' at index 1 -- matches the set {k,l,m,n}
    assert("alignas"s.find_first_of("klmn"s) == 1);

    // no character of "alignof" is in {w,x,y,z}
    assert("alignof"s.find_first_of("wxyz"s) == std::string::npos);

    // find_first_of("abc") looks for 'a', 'b', or 'c' -- NOT the
    // substring "abc" -- contrast with find, which looks for "abc" as
    // a whole
    assert("decltype"s.find("abc") == std::string::npos);  // no such substring
    assert("decltype"s.find_first_of("abc") == 2);          // 'c' at index 2
}
```

```text

```

### Reference

```cpp skip
size_type find_first_of( const basic_string& str, size_type pos = 0 ) const;  // (until C++11)
size_type find_first_of( const basic_string& str,
                         size_type pos = 0 ) const noexcept;  // (since C++11) (until C++20)
constexpr size_type find_first_of( const basic_string& str,
                                   size_type pos = 0 ) const noexcept;  // (since C++20)
size_type find_first_of( const CharT* s,
                         size_type pos, size_type count ) const;  // (until C++20)
constexpr size_type find_first_of( const CharT* s,
                                   size_type pos, size_type count ) const;  // (since C++20)
size_type find_first_of( const CharT* s, size_type pos = 0 ) const;  // (until C++20)
constexpr size_type find_first_of( const CharT* s, size_type pos = 0 ) const;  // (since C++20)
size_type find_first_of( CharT ch, size_type pos = 0 ) const;  // (until C++11)
size_type find_first_of( CharT ch, size_type pos = 0 ) const noexcept;  // (since C++11) (until C++20)
constexpr size_type find_first_of( CharT ch,
                                   size_type pos = 0 ) const noexcept;  // (since C++20)
template< class StringViewLike >
size_type
    find_first_of( const StringViewLike& t,
                   size_type pos = 0 ) const noexcept(/* see below */);  // (since C++17) (until C++20)
template< class StringViewLike >
constexpr size_type
    find_first_of( const StringViewLike& t,
                   size_type pos = 0 ) const noexcept(/* see below */);  // (since C++20)
```

The `StringViewLike` overload participates in overload resolution
only if `t` converts to `std::basic_string_view<CharT, Traits>` but
not to `const CharT*`. LWG 847 (C++98) added the strong exception
safety guarantee retroactively; LWG 2064 removed erroneous `noexcept`
from the C-string/char overloads in C++11, later restored by P1148R0.

### See also

- **find** — finds the first occurrence of a whole substring
- **rfind** — find the last occurrence of a substring
- **find_first_not_of** — find the first character *not* in a set
- **find_last_of** — find the last occurrence of any character in a set
- **find_last_not_of** — find the last character not in a set

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/find_first_of*
