# std::regex_constants::match_flag_type

```cpp
using match_flag_type = /* implementation-defined */;  // (1) (since C++11)
constexpr match_flag_type match_default =     {};
constexpr match_flag_type match_not_bol =     /* unspecified */;
constexpr match_flag_type match_not_eol =     /* unspecified */;
constexpr match_flag_type match_not_bow =     /* unspecified */;
constexpr match_flag_type match_not_eow =     /* unspecified */;
constexpr match_flag_type match_any =         /* unspecified */;
constexpr match_flag_type match_not_null =    /* unspecified */;
constexpr match_flag_type match_continuous =  /* unspecified */;
constexpr match_flag_type match_prev_avail =  /* unspecified */;
constexpr match_flag_type format_default =    {};
constexpr match_flag_type format_sed =        /* unspecified */;
constexpr match_flag_type format_no_copy =    /* unspecified */;
constexpr match_flag_type format_first_only = /* unspecified */;  // (since C++11) (until C++17)
inline constexpr match_flag_type match_default =     {};
inline constexpr match_flag_type match_not_bol =     /* unspecified */;
inline constexpr match_flag_type match_not_eol =     /* unspecified */;
inline constexpr match_flag_type match_not_bow =     /* unspecified */;
inline constexpr match_flag_type match_not_eow =     /* unspecified */;
inline constexpr match_flag_type match_any =         /* unspecified */;
inline constexpr match_flag_type match_not_null =    /* unspecified */;
inline constexpr match_flag_type match_continuous =  /* unspecified */;
inline constexpr match_flag_type match_prev_avail =  /* unspecified */;
inline constexpr match_flag_type format_default =    {};
inline constexpr match_flag_type format_sed =        /* unspecified */;
inline constexpr match_flag_type format_no_copy =    /* unspecified */;
inline constexpr match_flag_type format_first_only = /* unspecified */;  // (since C++17)
```

1) `match_flag_type` is a BitmaskType that specifies additional regular
   expression matching options.

### Constants

Note: `[``first``,``last``)` refers to the character sequence being matched.

- **`match_not_bol`** — The first character in `[``first``,``last``)` will be
  treated as if it is **not** at the beginning of a line (i.e. `^` will not
  match `[``first``,``first``)`).
- **`match_not_eol`** — The last character in `[``first``,``last``)` will be
  treated as if it is **not** at the end of a line (i.e. `$` will not match
  `[``last``,``last``)`).
- **`match_not_bow`** — `\b` will not match `[``first``,``first``)`.
- **`match_not_eow`** — `\b` will not match `[``last``,``last``)`.
- **`match_any`** — If more than one match is possible, then any match is an
  acceptable result.
- **`match_not_null`** — Do not match empty sequences.
- **`match_continuous`** — Only match a sub-sequence that begins at `first`.
- **`match_prev_avail`** — `--first` is a valid iterator position. When set,
  causes `match_not_bol` and `match_not_bow` to be ignored.
- **`format_default`** — Use ECMAScript rules to construct strings in
  `std::regex_replace` (syntax documentation).
- **`format_sed`** — Use POSIX *sed* utility rules in `std::regex_replace`
  (syntax documentation).
- **`format_no_copy`** — Do not copy un-matched strings to the output in
  `std::regex_replace`.
- **`format_first_only`** — Only replace the first match in
  `std::regex_replace`.

All constants, except for `match_default` and `format_default`, are bitmask
elements. The `match_default` and `format_default` constants are empty bitmasks.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2053 | C++11 | 1. the constants were declared static 2. `match_default`
      and `format_default` were initialized from `​0​` | 1. removed the static
      specifier 2. initialized from `{}`

### See also

- **regex_match (C++11)** — attempts to match a regular expression to an entire
  character sequence (function template)
- **syntax_option_type (C++11)** — general options controlling regex behavior
  (typedef)
- **error_type (C++11)** — describes different types of matching errors
  (typedef)

---
*Source: https://en.cppreference.com/w/cpp/regex/match_flag_type*
