# std::regex_traits

```cpp
template< class CharT >
class regex_traits;  // (since C++11)
```

The type trait template `regex_traits` supplies `std::basic_regex` with the set
of types and functions necessary to operate on the type `CharT`.

Since many of regex operations are locale-sensitive (when
`std::regex_constants::collate` flag is set), the regex_traits class typically
holds an instance of a `std::locale` as a private member.

### Standard specializations

Two specializations of `std::regex_traits` are defined by the standard library:

**`std::regex_traits<char>`**

**`std::regex_traits<wchar_t>`**

These specializations make it possible to use `std::basic_regex<char>` (aka
`std::regex`) and `std::basic_regex<wchar_t>` (aka `std::wregex`). To use
`std::basic_regex` with other character types (for example, `char32_t`), a
user-provided trait class must be used.

### Member types

- **`char_type`** — `CharT`
- **`string_type`** — `std::basic_string<CharT>`
- **`locale_type`** — The locale used for localized behavior in the regular
  expression. Must be CopyConstructible
- **`char_class_type`** — Represents a character classification and is capable
  of holding an implementation specific set returned by `lookup_classname`. Must
  be a BitmaskType.

### Member functions

- **(constructor)** — constructs the regex_traits object (public member
  function)
- **length [static]** — calculates the length of a null-terminated character
  string (public static member function)
- **translate** — determines the equivalence key for a character (public member
  function)
- **translate_nocase** — determines the case-insensitive equivalence key for a
  character (public member function)
- **transform** — determines the sort key for the given string, used to provide
  collation order (public member function)
- **transform_primary** — determines the primary sort key for the character
  sequence, used to determine equivalence class (public member function)
- **lookup_collatename** — gets a collation element by name (public member
  function)
- **lookup_classname** — gets a character class by name (public member function)
- **isctype** — indicates membership in a localized character class (public
  member function)
- **value** — translates the character representing a numeric digit into an
  integral value (public member function)
- **imbue** — sets the locale (public member function)
- **getloc** — gets the locale (public member function)

---
*Source: https://en.cppreference.com/w/cpp/regex/regex_traits*
