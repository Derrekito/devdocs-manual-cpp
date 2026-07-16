# std::basic_string<CharT,Traits,Allocator>::replace

```cpp
basic_string& replace( size_type pos, size_type count,
                       const basic_string& str );  // (1) (constexpr since C++20)
basic_string& replace( const_iterator first, const_iterator last,
                       const basic_string& str );  // (2) (constexpr since C++20)
basic_string& replace( size_type pos, size_type count,
                       const basic_string& str,
                       size_type pos2, size_type count2 );  // (until C++14)
basic_string& replace( size_type pos, size_type count,
                       const basic_string& str,
                       size_type pos2, size_type count2 = npos );  // (since C++14) (constexpr since C++20)
basic_string& replace( size_type pos, size_type count,
                       const CharT* cstr, size_type count2 );  // (4) (constexpr since C++20)
basic_string& replace( const_iterator first, const_iterator last,
                       const CharT* cstr, size_type count2 );  // (5) (constexpr since C++20)
basic_string& replace( size_type pos, size_type count,
                       const CharT* cstr );  // (6) (constexpr since C++20)
basic_string& replace( const_iterator first, const_iterator last,
                       const CharT* cstr );  // (7) (constexpr since C++20)
basic_string& replace( size_type pos, size_type count,
                       size_type count2, CharT ch );  // (8) (constexpr since C++20)
basic_string& replace( const_iterator first, const_iterator last,
                       size_type count2, CharT ch );  // (9) (constexpr since C++20)
template< class InputIt >
basic_string& replace( const_iterator first, const_iterator last,
                       InputIt first2, InputIt last2 );  // (10) (constexpr since C++20)
basic_string& replace( const_iterator first, const_iterator last,
                       std::initializer_list<CharT> ilist );  // (11) (since C++11) (constexpr since C++20)
template< class StringViewLike >
basic_string& replace( size_type pos, size_type count,
                       const StringViewLike& t );  // (12) (since C++17) (constexpr since C++20)
template< class StringViewLike >
basic_string& replace( const_iterator first, const_iterator last,
                       const StringViewLike& t );  // (13) (since C++17) (constexpr since C++20)
template< class StringViewLike >
basic_string& replace( size_type pos, size_type count,
                       const StringViewLike& t,
                       size_type pos2, size_type count2 = npos );  // (14) (since C++17) (constexpr since C++20)
```

Replaces the characters in the range `[``begin() + pos``,``begin() +
std::min(pos + count, size())``)` or `[``first``,``last``)` with given
characters.

1,2) Those characters are replaced with `str`.

3) Those characters are replaced with a substring `[``pos2``,``std::min(pos2 +
   count2, str.size())``)` of `str`.

4,5) Those characters are replaced with the characters in the range
   `[``cstr``,``cstr + count2``)`.

If `[``cstr``,``cstr + count2``)` is not a valid range, the behavior is
   undefined.

6,7) Those characters are replaced with the characters in the range
   `[``cstr``,``cstr + Traits::length(cstr)``)`.

8,9) Those characters are replaced with `count2` copies of `ch`.

10) Those characters are replaced with the characters in the range
   `[``first2``,``last2``)` as if by `replace(first, last, basic_string(first2,
   last2, get_allocator()))`.

11) Those characters are replaced with the characters in `ilist`.

12,13) Implicitly converts `t` to a string view `sv` as if by
   `std::basic_string_view<CharT, Traits> sv = t;`, then those characters are
   replaced with the characters from `sv`.

These overloads participate in overload resolution only if
   `std::is_convertible_v<const StringViewLike&, std::basic_string_view<CharT,
   Traits>>` is `true` and `std::is_convertible_v<const StringViewLike&, const
   CharT*>` is `false`.

14) Implicitly converts `t` to a string view `sv` as if by
   `std::basic_string_view<CharT, Traits> sv = t;`, then those characters are
   replaced with the characters from the subview `sv.substr(pos2, count2)`.

This overload participates in overload resolution only if
   `std::is_convertible_v<const StringViewLike&, std::basic_string_view<CharT,
   Traits>>` is `true` and `std::is_convertible_v<const StringViewLike&, const
   CharT*>` is `false`.

If `[``begin()``,``first``)` or `[``first``,``last``)` is not a valid range, the
behavior is undefined.

### Parameters

- **pos** — start of the substring that is going to be replaced
- **count** — length of the substring that is going to be replaced
- **first, last** — range of characters that is going to be replaced
- **str** — string to use for replacement
- **pos2** — start of the substring to replace with
- **count2** — number of characters to replace with
- **cstr** — pointer to the character string to use for replacement
- **ch** — character value to use for replacement
- **first2, last2** — range of characters to use for replacement
- **ilist** — initializer list with the characters to use for replacement
- **t** — object (convertible to `std::basic_string_view`) with the characters
  to use for replacement

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

### Return value

`*this`.

### Exceptions

1) Throws `std::out_of_range` if `pos > size()`.

3) Throws `std::out_of_range` if `pos > size()` or `pos2 > str.size()`.

4,6,8) Throws `std::out_of_range` if `pos > size()`.

12,14) Throws `std::out_of_range` if `pos > size()`.

If the operation would result in `size() > max_size()`, throws
`std::length_error`.

If an exception is thrown for any reason, these functions have no effect (strong
exception safety guarantee).

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 847 | C++98 | there was no exception safety guarantee | added strong
      exception safety guarantee
  LWG 1323 | C++98 | the types of `first` and `last` were `iterator` | changed
      to `const_iterator`
  LWG 2946 | C++17 | overloads (12,13) caused ambiguity in some cases | avoided
      by making them templates

### See also

- **replace_with_range (C++23)** — replaces specified portion of a string with a
  range of characters (public member function)
- **regex_replace (C++11)** — replaces occurrences of a regular expression with
  formatted replacement text (function template)
- **replacereplace_if** — replaces all values satisfying specific criteria with
  another value (function template)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/replace*
