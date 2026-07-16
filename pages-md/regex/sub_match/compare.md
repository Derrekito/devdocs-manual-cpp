# std::sub_match<BidirIt>::compare

```cpp
int compare( const sub_match& m ) const;  // (1) (since C++11)
int compare( const string_type& s ) const;  // (2) (since C++11)
int compare( const value_type* c ) const;  // (3) (since C++11)
```

1) Compares two `sub_match` directly by comparing their underlying character
   sequences. Equivalent to `str().compare(m.str())`.

2) Compares a `sub_match` with a `std::basic_string`. Equivalent to
   `str().compare(s)`.

3) Compares a `sub_match` with a null-terminated sequence of the underlying
   character type pointed to by `s`. Equivalent to `str().compare(c)`.

This function is infrequently used directly by application code. Instead, one of
the non-member comparison operators is used.

### Parameters

- **m** — a reference to another sub_match
- **s** — a reference to a string to compare to
- **c** — a pointer to a null-terminated character sequence of the underlying
  `value_type` to compare to

### Return value

A value less than zero if this `sub_match` is *less* than the other character
sequence, zero if the both underlying character sequences are equal, greater
than zero if this `sub_match` is *greater* than the other character sequence.

### Example

### See also

- **compare** — compares two strings (public member function of
  `std::basic_string<CharT,Traits,Allocator>`)
- **stroperator string_type** — converts to the underlying string type (public
  member function)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — compares a `sub_match` with another
  `sub_match`, a string, or a character (function template)

---
*Source: https://en.cppreference.com/w/cpp/regex/sub_match/compare*
