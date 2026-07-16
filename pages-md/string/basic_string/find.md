# std::basic_string<CharT,Traits,Allocator>::find

```cpp
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

Finds the first substring equal to the given character sequence. Search begins
at `pos`, i.e. the found substring must not begin in a position preceding `pos`.

1) Finds the first substring equal to `str`.

2) Finds the first substring equal to the range `[``s``,``s + count``)`. This
   range may contain null characters.

If `[``s``,``s + count``)` is not a valid range, the behavior is undefined.

3) Finds the first substring equal to the character string pointed to by `s`.
   The length of the string is determined by the first null character using
   `Traits::length(s)`.

If `[``s``,``s + Traits::length(s)``)` is not a valid range, the behavior is
   undefined.

4) Finds the first character `ch` (treated as a single-character substring by
   the formal rules below).

5) Implicitly converts `t` to a string view `sv` as if by
   `std::basic_string_view<CharT, Traits> sv = t;`, then finds the first
   substring equal to `sv`.

This overload participates in overload resolution only if
   `std::is_convertible_v<const StringViewLike&, std::basic_string_view<CharT,
   Traits>>` is `true` and `std::is_convertible_v<const StringViewLike&, const
   CharT*>` is `false`.

Formally, a substring `str` is said to be *found* at position `xpos` if all of
the following are `true`:

- `xpos >= pos`
- `xpos + str.size() <= size()`
- for all positions `n` in `str`, `Traits::eq(at(xpos + n), str.at(n))`.

In particular, this implies that

- a substring can be found only if `pos <= size() - str.size()`
- an empty substring is found at `pos` if and only if `pos <= size()`
- for a non-empty substring, if `pos >= size()`, the function always returns
  `npos`.

### Parameters

- **str** — string to search for
- **pos** — position at which to start the search
- **count** — length of substring to search for
- **s** — pointer to a character string to search for
- **ch** — character to search for
- **t** — object (convertible to `std::basic_string_view`) to search for

### Return value

Position of the first character of the found substring or `npos` if no such
substring is found.

### Exceptions

1,4) Throws nothing.

5) `noexcept` specification: `noexcept(std::is_nothrow_convertible_v<const T&,
   std::basic_string_view<CharT, Traits>>)`

If an exception is thrown for any reason, this function has no effect (strong
exception safety guarantee).

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <string>

void print(int id, std::string::size_type n, std::string const& s)
{
    std::cout << id << ") ";
    if (std::string::npos == n)
        std::cout << "not found! n == npos\n";
    else
        std::cout << "found @ n = " << n << ", substr(" << n << ") = "
                  << std::quoted(s.substr(n)) << '\n';
}

int main()
{
    std::string::size_type n;
    std::string const s = "This is a string"; /*
                             ^  ^  ^
                             1  2  3          */

    // search from beginning of string
    n = s.find("is");
    print(1, n, s);

    // search from position 5
    n = s.find("is", 5);
    print(2, n, s);

    // find a single character
    n = s.find('a');
    print(3, n, s);

    // find a single character
    n = s.find('q');
    print(4, n, s);
}
```

Output:

```text
1) found @ n = 2, substr(2) = "is is a string"
2) found @ n = 5, substr(5) = "is a string"
3) found @ n = 8, substr(8) = "a string"
4) not found! n == npos
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 847 | C++98 | there was no exception safety guarantee | added strong
      exception safety guarantee
  LWG 2064 | C++11 | overloads (3,4) were noexcept | removed
  LWG 2946 | C++17 | overload (5) caused ambiguity in some cases | avoided by
      making it a template
  P1148R0 | C++11 C++17 | noexcept for overloads (4,5) were accidently dropped
      by LWG2064/LWG2946 | restored

### See also

- **strstr** — finds the first occurrence of a substring of characters
  (function)
- **wcsstr** — finds the first occurrence of a wide string within another wide
  string (function)
- **strchr** — finds the first occurrence of a character (function)
- **wcschr** — finds the first occurrence of a wide character in a wide string
  (function)
- **rfind** — find the last occurrence of a substring (public member function)
- **find_first_of** — find first occurrence of characters (public member
  function)
- **find_first_not_of** — find first absence of characters (public member
  function)
- **find_last_of** — find last occurrence of characters (public member function)
- **find_last_not_of** — find last absence of characters (public member
  function)
- **find** — find characters in the view (public member function of
  `std::basic_string_view<CharT,Traits>`)
- **search** — searches for a range of elements (function template)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/find*
