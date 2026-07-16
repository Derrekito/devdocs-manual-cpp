# std::filesystem::path::assign

```cpp
path& assign( string_type&& source );  // (1) (since C++17)
template< class Source >
path& assign( const Source& source );  // (2) (since C++17)
template< class InputIt >
path& assign( InputIt first, InputIt last );  // (3) (since C++17)
```

Replaces the contents to the `path` object by a new pathname constructed from
the given character sequence.

1) Assigns the pathname identified by the detected-format string `source`, which
   is left in valid, but unspecified state.

2) Assigns the pathname identified by the detected-format character range
   `source`.

3) Assigns the pathname identified by detected-format character range `[first,
   last)`.

(2) participates in overload resolution only if `Source` and `path` are not the
same type, and either:

- `Source` is a specialization of `std::basic_string` or
  `std::basic_string_view`, or
- `std::iterator_traits<std::decay_t<Source>>::value_type` is valid and denotes
  a possibly const-qualified encoding character type (`char`, `char8_t`, (since
  C++20)`char16_t`, `char32_t`, or `wchar_t`).

### Parameters

- **source** — a character range to use, represented as `std::string`,
  `std::string_view`, pointer to a null-terminated multibyte string, or as an
  input iterator with char value type that points to a null-terminated multibyte
  string
- **first, last** — a character range to use

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-The value type of `InputIt` must be one of the encoded character types (`char`, `wchar_t`, `char16_t` and `char32_t`)**

### Return value

`*this`

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3244 | C++17 | constraint that `Source` cannot be `path` was missing |
      added

### See also

- **operator=** — assigns another path (public member function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/assign*
