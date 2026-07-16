# std::basic_string<CharT,Traits,Allocator>::find_last_not_of

```cpp
size_type find_last_not_of( const basic_string& str,
                            size_type pos = npos ) const;  // (until C++11)
size_type find_last_not_of( const basic_string& str,
                            size_type pos = npos ) const noexcept;  // (since C++11) (until C++20)
constexpr size_type find_last_not_of( const basic_string& str,
                                      size_type pos = npos ) const noexcept;  // (since C++20)
size_type find_last_not_of( const CharT* s,
                            size_type pos, size_type count ) const;  // (until C++20)
constexpr size_type find_last_not_of( const CharT* s,
                                      size_type pos, size_type count ) const;  // (since C++20)
size_type find_last_not_of( const CharT* s, size_type pos = npos ) const;  // (until C++20)
constexpr size_type find_last_not_of( const CharT* s,
                                      size_type pos = npos ) const;  // (since C++20)
size_type find_last_not_of( CharT ch, size_type pos = npos ) const;  // (until C++11)
size_type find_last_not_of( CharT ch, size_type pos = npos ) const noexcept;  // (since C++11) (until C++20)
constexpr size_type find_last_not_of( CharT ch,
                                      size_type pos = npos ) const noexcept;  // (since C++20)
template< class StringViewLike >
size_type
    find_last_not_of( const StringViewLike& t,
                      size_type pos = npos ) const noexcept(/* see below */);  // (since C++17) (until C++20)
template< class StringViewLike >
constexpr size_type
    find_last_not_of( const StringViewLike& t,
                      size_type pos = npos ) const noexcept(/* see below */);  // (since C++20)
```

Finds the last character equal to none of the characters in the given character
sequence. The search considers only the range `[``​0​``,``pos``]`. If all
characters in the range can be found in the given character sequence, `npos`
will be returned.

1) Finds the last character equal to none of characters in `str`.

2) Finds the last character equal to none of characters in the range `[``s``,``s
   + count``)`. This range can include null characters.

If `[``s``,``s + count``)` is not a valid range, the behavior is undefined.

3) Finds the last character equal to none of characters in character string
   pointed to by `s`. The length of the string is determined by the first null
   character using `Traits::length(s)`.

If `[``s``,``s + Traits::length(s)``)` is not a valid range, the behavior is
   undefined.

4) Finds the last character not equal to `ch`.

5) Implicitly converts `t` to a string view `sv` as if by
   `std::basic_string_view<CharT, Traits> sv = t;`, then finds the last
   character equal to none of characters in `sv`.

This overload participates in overload resolution only if
   `std::is_convertible_v<const StringViewLike&, std::basic_string_view<CharT,
   Traits>>` is `true` and `std::is_convertible_v<const StringViewLike&, const
   CharT*>` is `false`.

In all cases, equality is checked by calling `Traits::eq`.

### Parameters

- **str** — string identifying characters to search for
- **pos** — position at which to end search
- **count** — length of character string identifying characters to search for
- **s** — pointer to a character string identifying characters to search for
- **ch** — character identifying characters to search for
- **t** — object (convertible to `std::basic_string_view`) identifying
  characters to search for

### Return value

Position of the found character or `npos` if no such character is found.

### Exceptions

1,4) Throws nothing.

5) `noexcept` specification: `noexcept(std::is_nothrow_convertible_v< const T&,
   std::basic_string_view<CharT, Traits>>)`

If an exception is thrown for any reason, this function has no effect (strong
exception safety guarantee).

### Example

```cpp
#include <iostream>
#include <string>

void show_pos(const std::string& str, std::string::size_type found)
{
    if (found != std::string::npos)
        std::cout << '[' << found << "] = \'" << str[found] << "\'\n";
    else
        std::cout << "not found\n";
}

int main()
{
    std::string str{"abc_123"};
    char const* skip_set{"0123456789"};
    std::string::size_type str_last_pos{std::string::npos};

    show_pos(str, str.find_last_not_of(skip_set)); // [3] = '_'

    str_last_pos = 2;
    show_pos(str, str.find_last_not_of(skip_set, str_last_pos)); // [2] = 'c'

    str_last_pos = 2;
    show_pos(str, str.find_last_not_of('c', str_last_pos)); // [1] = 'b'

    const char arr[]{'3', '4', '5'};
    show_pos(str, str.find_last_not_of(arr)); // [5] = '2'

    str_last_pos = 2;
    std::string::size_type skip_set_size{4};
    show_pos(str, str.find_last_not_of(skip_set,
                                       str_last_pos,
                                       skip_set_size)); // [2] = 'c'

    show_pos(str, str.find_last_not_of("abc")); // [6] = '3'

    str_last_pos = 2;
    show_pos(str, str.find_last_not_of("abc", str_last_pos)); // not found
}
```

Output:

```text
[3] = '_'
[2] = 'c'
[1] = 'b'
[5] = '2'
[2] = 'c'
[6] = '3'
not found
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 141 | C++98 | overload (1) could only return `npos` if `pos >= size()` |
      the search range is `[``​0​``,``size()``)` in this case
  LWG 847 | C++98 | there was no exception safety guarantee | added strong
      exception safety guarantee
  LWG 2064 | C++11 | overloads (3,4) were noexcept | removed
  LWG 2946 | C++17 | overload (5) caused ambiguity in some cases | avoided by
      making it a template
  P1148R0 | C++11 C++17 | noexcept for overloads (4,5) were accidently dropped
      by LWG2064/LWG2946 | restored

### See also

- **find** — finds the first occurrence of the given substring (public member
  function)
- **rfind** — find the last occurrence of a substring (public member function)
- **find_first_of** — find first occurrence of characters (public member
  function)
- **find_first_not_of** — find first absence of characters (public member
  function)
- **find_last_of** — find last occurrence of characters (public member function)
- **find_last_not_of** — find last absence of characters (public member function
  of `std::basic_string_view<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/find_last_not_of*
