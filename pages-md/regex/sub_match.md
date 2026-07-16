# std::sub_match

```cpp
template< class BidirIt >
class sub_match;  // (since C++11)
```

The class template `std::sub_match` is used by the regular expression engine to
denote sequences of characters matched by marked sub-expressions. A match is a
`[``begin``,``end``)` pair within the target range matched by the regular
expression, but with additional observer functions to enhance code clarity.

Only the default constructor is publicly accessible. Instances of
`std::sub_match` are normally constructed and populated as a part of a
`std::match_results` container during the processing of one of the regex
algorithms.

The member functions return defined default values unless the `matched` member
is `true`.

`std::sub_match` inherits from std::pair<BidirIt, BidirIt>, although it cannot
be treated as a `std::pair` object because member functions such as assignment
will not work as expected.

### Type requirements

**-`BidirIt` must meet the requirements of LegacyBidirectionalIterator.**

### Specializations

Several specializations for common character sequence types are provided:

- **`std::csub_match`** — std::sub_match<const char*>
- **`std::wcsub_match`** — std::sub_match<const wchar_t*>
- **`std::ssub_match`** — std::sub_match<std::string::const_iterator>
- **`std::wssub_match`** — std::sub_match<std::wstring::const_iterator>

### Member types

- **`iterator`** — `BidirIt`
- **`value_type`** — std::iterator_traits<BidirIt>::value_type
- **`difference_type`** — std::iterator_traits<BidirIt>::difference_type
- **`string_type`** — std::basic_string<value_type>

### Member objects

- **bool matched** — Indicates if this match was successful (public member
  object)

## Inherited from std::pair

- **BidirIt first** — Start of the match sequence. (public member object)
- **BidirIt second** — One-past-the-end of the match sequence. (public member
  object)

### Member functions

- **(constructor)** — constructs the match object (public member function)

**Observers**

- **length** — returns the length of the match (if any) (public member function)
- **stroperator string_type** — converts to the underlying string type (public
  member function)
- **compare** — compares matched subsequence (if any) (public member function)

### Non-member functions

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — compares a `sub_match` with another
  `sub_match`, a string, or a character (function template)
- **operator<<** — outputs the matched character subsequence (function template)

### See also

- **regex_token_iterator (C++11)** — iterates through the specified
  sub-expressions within all regex matches in a given string or through
  unmatched substrings (class template)

---
*Source: https://en.cppreference.com/w/cpp/regex/sub_match*
