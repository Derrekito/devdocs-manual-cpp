# std::basic_string<CharT,Traits,Allocator>::append

Appends characters to the end of the string and returns `*this`, so
calls can be chained. For the common case of "add this whole string
or literal to the end," prefer the terser `operator+=`; reach for
`append` when you need a repeat count, a substring, an iterator range,
or a `pos`/`count` slice of the source.

```cpp skip
s.append(count, ch);          // count copies of a character
s.append(str);                // another string, in full
s.append(str, pos, count);    // a substring of another string
s.append(cstr, count);        // count chars from a C string (may hold '\0')
s.append(cstr);               // a null-terminated C string
s.append(first, last);        // an iterator range
s.append({a, b, c});          // an initializer list           (since C++11)
s.append(string_view_like);   // anything convertible to string_view (since C++17)
```

### What you provide

- **count, ch** — repeat `ch` this many times.
- **str, pos, count** — another string, or the substring
  `[pos, pos + count)` of it; an oversized `count` (or `npos`) clamps
  to the end of `str`; `pos > str.size()` throws `std::out_of_range`.
- **cstr, count** — `count` characters starting at `cstr`, which may
  include embedded nulls; without `count`, it's read as a
  null-terminated C string via `Traits::length`.
- **first, last** — any LegacyInputIterator range.
- **ilist** — a braced list of characters (since C++11).
- **t** — anything implicitly convertible to `std::basic_string_view`
  (since C++17), optionally sliced with `pos`/`count`.

### Guarantees and costs

- Returns `*this`, enabling `s.append(a).append(b)` chains.
- No standard complexity guarantee; typical implementations behave
  like `std::vector::insert` — amortized linear in the appended
  length, with occasional reallocation.
- Throws `std::length_error` if the result would exceed `max_size()`.
  Strong exception safety: on throw, the string is left unchanged.

### Gotchas

- `append(str, pos, count)` throws only if `pos > str.size()`; an
  oversized `count` is silently clamped, not an error.
- The `(cstr, count)` overload can contain embedded nulls; the plain
  `(cstr)` overload stops at the first `'\0'` — pick the wrong one and
  you either truncate or read past your intended data.
- Appending invalidates iterators/pointers/references into the string
  whenever a reallocation happens — don't hold onto them across a call
  you haven't sized for.

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    std::string str = "string";
    const char* cptr = "C-string";

    std::string output;

    output.append(3, '*');           // 1) three '*' characters
    std::cout << "1) " << output << '\n';

    output.append(str);              // 2) a whole string
    std::cout << "2) " << output << '\n';

    output.append(str, 3, 3);        // 3) substring "ing"
    std::cout << "3) " << output << '\n';

    output.append(1, ' ').append(cptr);  // 4) chained calls
    std::cout << "4) " << output << '\n';
}
```

```text
1) ***
2) ***string
3) ***stringing
4) ***stringing C-string
```

### Reference

```cpp skip
basic_string& append( size_type count, CharT ch );
basic_string& append( const basic_string& str );
basic_string& append( const basic_string& str,
                      size_type pos, size_type count = npos );  // count default since C++14
basic_string& append( const CharT* s, size_type count );
basic_string& append( const CharT* s );
template< class InputIt >
basic_string& append( InputIt first, InputIt last );
basic_string& append( std::initializer_list<CharT> ilist );  // (since C++11)
template< class StringViewLike >
basic_string& append( const StringViewLike& t );  // (since C++17)
template< class StringViewLike >
basic_string& append( const StringViewLike& t,
                      size_type pos, size_type count = npos );  // (since C++17)
```

All overloads are `constexpr` since C++20. The `StringViewLike`
overloads participate in overload resolution only if `t` converts to
`std::basic_string_view<CharT, Traits>` but not to `const CharT*`. The
iterator overload behaves like the `(count, ch)` overload if `InputIt`
is an integral type (until C++11); since C++11 it participates only
if `InputIt` is a genuine LegacyInputIterator.

### See also

- **append_range** — appends a range of characters (C++23)
- **operator+=** — the terser append for whole strings/literals
- **replace** — replaces a slice with new characters
- **push_back** — appends a single character

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/append*
