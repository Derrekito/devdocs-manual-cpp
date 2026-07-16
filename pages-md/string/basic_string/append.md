# std::basic_string<CharT,Traits,Allocator>::append

```cpp
basic_string& append( size_type count, CharT ch );  // (until C++20)
constexpr basic_string& append( size_type count, CharT ch );  // (since C++20)
basic_string& append( const basic_string& str );  // (until C++20)
constexpr basic_string& append( const basic_string& str );  // (since C++20)
basic_string& append( const basic_string& str,
                      size_type pos, size_type count );  // (until C++14)
basic_string& append( const basic_string& str,
                      size_type pos, size_type count = npos );  // (since C++14) (until C++20)
constexpr basic_string& append( const basic_string& str,
                                size_type pos, size_type count = npos );  // (since C++20)
basic_string& append( const CharT* s, size_type count );  // (until C++20)
constexpr basic_string& append( const CharT* s, size_type count );  // (since C++20)
basic_string& append( const CharT* s );  // (until C++20)
constexpr basic_string& append( const CharT* s );  // (since C++20)
template< class InputIt >
basic_string& append( InputIt first, InputIt last );  // (until C++20)
template< class InputIt >
constexpr basic_string& append( InputIt first, InputIt last );  // (since C++20)
basic_string& append( std::initializer_list<CharT> ilist );  // (since C++11) (until C++20)
constexpr basic_string& append( std::initializer_list<CharT> ilist );  // (since C++20)
template< class StringViewLike >
basic_string& append( const StringViewLike& t );  // (since C++17) (until C++20)
template< class StringViewLike >
constexpr basic_string& append( const StringViewLike& t );  // (since C++20)
template< class StringViewLike >
basic_string& append( const StringViewLike& t,
                      size_type pos, size_type count = npos );  // (since C++17) (until C++20)
template< class StringViewLike >
constexpr basic_string& append( const StringViewLike& t,
                                size_type pos, size_type count = npos );  // (since C++20)
```

Appends additional characters to the string.

1) Appends `count` copies of character `ch`.

2) Appends string `str`.
3) Appends a substring `[``pos``,``pos + count``)` of `str`.

- If the requested substring lasts past the end of the string, or if `count ==
  npos`, the appended substring is `[``pos``,``size()``)`.
- If `pos > str.size()`, `std::out_of_range` is thrown.

4) Appends characters in the range `[``s``,``s + count``)`. This range can
   contain null characters.

If `[``s``,``s + count``)` is not a valid range, the behavior is undefined.

5) Appends the null-terminated character string pointed to by `s`, as if by
   `append(s, Traits::length(s))`.

6) Appends characters in the range `[``first``,``last``)`. This overload has the
   same effect as overload (1) if `InputIt` is an integral type. (until C++11)
   This overload only participates in overload resolution if `InputIt` qualifies
   as a LegacyInputIterator. (since C++11)

7) Appends characters from the initializer list `ilist`.

8) Implicitly converts `t` to a string view `sv` as if by
   `std::basic_string_view<CharT, Traits> sv = t;`, then appends all characters
   from `sv` as if by `append(sv.data(), sv.size())`.

This overload participates in overload resolution only if
   `std::is_convertible_v<const StringViewLike&, std::basic_string_view<CharT,
   Traits>>` is `true` and `std::is_convertible_v<const StringViewLike&, const
   CharT*>` is `false`.
9) Implicitly converts `t` to a string view `sv` as if by
`std::basic_string_view<CharT, Traits> sv = t;`, then appends the characters
from the subview `[``pos``,``pos + count``)` of `sv`.

- If the requested subview extends past the end of `sv`, or if `count == npos`,
  the appended subview is `[``pos``,``sv.size()``)`.
- If `pos >= sv.size()`, `std::out_of_range` is thrown.

This overload participates in overload resolution only if
   `std::is_convertible_v<const StringViewLike&, std::basic_string_view<CharT,
   Traits>>` is `true` and `std::is_convertible_v<const StringViewLike&, const
   CharT*>` is `false`.

### Parameters

- **count** — number of characters to append
- **pos** — the index of the first character to append
- **ch** — character value to append
- **first, last** — range of characters to append
- **str** — string to append
- **s** — pointer to the character string to append
- **ilist** — initializer list with the characters to append
- **t** — object convertible to `std::basic_string_view` with the characters to
  append

### Return value

`*this`

### Complexity

There are no standard complexity guarantees, typical implementations behave
similar to `std::vector::insert()`.

### Exceptions

If the operation would result in `size() > max_size()`, throws
`std::length_error`.

If an exception is thrown for any reason, this function has no effect (strong
exception safety guarantee).

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    std::basic_string<char> str = "string";
    const char* cptr = "C-string";
    const char carr[] = "Two and one";

    std::string output;

    // 1) Append a char 3 times.
    // Notice, this is the only overload accepting chars.
    output.append(3, '*');
    std::cout << "1) " << output << '\n';

    // 2) Append a whole string
    output.append(str);
    std::cout << "2) " << output << '\n';

    // 3) Append part of a string (last 3 letters, in this case)
    output.append(str, 3, 3);
    std::cout << "3) " << output << '\n';

    // 4) Append part of a C-string
    // Notice, because `append` returns *this, we can chain calls together
    output.append(1, ' ').append(carr, 4);
    std::cout << "4) " << output << '\n';

    // 5) Append a whole C-string
    output.append(cptr);
    std::cout << "5) " << output << '\n';

    // 6) Append range
    output.append(&carr[3], std::end(carr));
    std::cout << "6) " << output << '\n';

    // 7) Append initializer list
    output.append({' ', 'l', 'i', 's', 't'});
    std::cout << "7) " << output << '\n';
}
```

Output:

```text
1) ***
2) ***string
3) ***stringing
4) ***stringing Two
5) ***stringing Two C-string
6) ***stringing Two C-string and one
7) ***stringing Two C-string and one list
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 847 | C++98 | there was no exception safety guarantee | added strong
      exception safety guarantee
  LWG 2946 | C++17 | overload (8) causes ambiguity in some cases | avoided by
      making it a template

### See also

- **append_range (C++23)** — appends a range of characters to the end (public
  member function)
- **operator+=** — appends characters to the end (public member function)
- **strcat** — concatenates two strings (function)
- **strncat** — concatenates a certain amount of characters of two strings
  (function)
- **wcscat** — appends a copy of one wide string to another (function)
- **wcsncat** — appends a certain amount of wide characters from one wide string
  to another (function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/append*
