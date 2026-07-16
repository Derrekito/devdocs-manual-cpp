# C++ named requirements: RegexTraits (since C++11)

**RegexTraits** is a traits class that abstracts operations needed for a given
character type when as template type parameters for `std::basic_regex`.

### Requirements

Given

- `CharT`, a character type
- `X`, a RegexTraits type for type `CharT`
- `u`, an object of type `X`
- `v`, an object of type `const X`
- `p`, a value of type `const CharT*`
- `I1, I2`, input iterators
- `F1, F2`, forward iterators
- `c`, a value of type `const CharT`
- `s`, an object of type `X::string_type`
- `cs`, an object of type `const X::string_type`
- `b`, a value of type `bool`
- `I`, a value of type `int`
- `cl`, an object of type `X::char_class_type`
- `loc`, an object of type `X::locale_type`

  expression | return type | semantics
  `X::char_type` | `CharT` | Used to refer to the character type.
  `X::string_type` | `std::basic_string<CharT>`
  `X::locale_type` | A CopyConstructible type that represents the locale used by
      the traits class.
  `X::char_class_type` | A BitmaskType type that represents a particular
      character classification.
  `X::length(p)` | `std::size_t` | Returns: the smallest `i` such that `p[i] ==
      0`. Complexity is linear in `i`.
  `v.translate(c)` | `X::char_type` | Returns: a character such that for any
      character `d` considered to be equivalent to `c`, then `v.translate(c) ==
      v.translate(d)`.
  `v.translate_nocase(c)` | `X::char_type` | Returns: a character such that for
      any character `C` considered to be equivalent to `c` without regard to
      case, then `v.translate_nocase(c) == v.translate_nocase(C)`.
  `v.transform(F1, F2)` | `X::string_type` | Returns: a sort key for the
      character sequence designated by the iterator range `[F1, F2)` such that
      if the character sequence `[G1, G2)` sorts before the character sequence
      `[H1, H2)`, then `v.transform(G1, G2) < v.transform(H1, H2)`.
  `v.transform_primary(F1, F2)` | `X::string_type` | Returns: a sort key for the
      character sequence designated by the iterator range `[F1, F2)` such that
      if the character sequence `[G1, G2)` sorts before the character sequence
      `[H1, H2)` without regard to case, then `v.transform_primary(G1, G2) <
      v.transform_primary(H1, H2)`.
  `v.lookup_collatename(F1, F2)` | `X::string_type` | Returns: An empty string
      if the character sequence is not a valid collating element A sequence of
      characters that represents the collating element consisting of the
      character sequence designated by the iterator range `[F1, F2)` otherwise
  `v.lookup_classname(F1, F2, b)` | `X::char_class_type` | Converts the
      character sequence designated by the iterator range `[F1, F2)` into a
      value of a BitmaskType that can subsequently be passed to `isctype` Values
      returned from `lookup_classname` can be bitwise or'ed together; the
      resulting value represents membership in either of the corresponding
      character classes If `b` is true, the returned bitmask is suitable for
      matching characters without regard to their case. Returns `​0​` if the
      character sequence is not the name of a character class recognized by `X`.
      The value returned shall be independent of the case of the characters in
      the sequence.
  `v.isctype(c, cl)` | `bool` | Returns: whether `c` is a member of one of the
      character classes designated by `cl` or not.
  `v.value(c, I)` | `int` | Returns: The value represented by the digit `c` in
      base `I` if the character `c` is a valid digit in base `I` `-1` otherwise
  `u.imbue(loc)` | `X::locale_type` | Imbues `u` with the locale `loc` Returns:
      The previous locale used by `u` if any
  `v.getloc()` | `X::locale_type` | Returns: The previous locale used by `v` if
      any.

### Standard library

RegexTraits is satisfied by the following standard library classes:

```cpp
template<> class regex_traits<char>;
template<> class regex_traits<wchar_t>;  // (since C++11)(since C++11)
```

---
*Source: https://en.cppreference.com/w/cpp/named_req/RegexTraits*
