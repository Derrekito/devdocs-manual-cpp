# std::range_formatter

```cpp
template< class T, class CharT = char >
  requires std::same_as<std::remove_cvref_t<T>, T> && std::formattable<T, CharT>
class range_formatter;  // (since C++23)
```

The `std::range_formatter` is a helper class template for implementing
`std::formatter` specializations for range types.

### Range format specification

The syntax of range-format-spec is:

```text
range-fill-and-align ﻿(optional) width ﻿(optional) n(optional) range-type ﻿(optional) range-underlying-spec ﻿(optional)
```

The range-fill-and-align is interpreted the same way as a fill-and-align except
that the fill in range-fill-and-align is any character other than `{`, `}`, or
`:`.

The width is described in standard format width specification.

The `n` option causes the range to be formatted without the opening and closing
brackets.

The format-spec in a range-underlying-spec (its syntax is equivalent to `:`
format-spec), if any, is interpreted by the range element formatter
`std::formatter<T, CharT>`.

The range-type changes the way a range is formatted, with certain options only
valid with certain argument types.

The available range presentation types are:

- `m`: Indicates that the opening bracket should be `"{"`, the closing bracket
  should be `"}"`, the separator should be `", "`, and each range element should
  be formatted as if `m` were specified for its tuple-type (in
  tuple-format-spec).
- If `m` is chosen as the range-type, the program is ill-formed unless `T` is
  either a specialization of:
- `s`: Indicates that the range should be formatted as a string.
- `?s`: Indicates that the range should be formatted as an escaped string.

### Member objects

- **`underlying_` (private)** — the underlying formatter of type
  `std::formatter<T, CharT>` (exposition-only member object*)
- **`separator_` (private)** — a string representing the separator of the range
  formatted result (the default string value is `", "`) (exposition-only member
  object*)
- **`opening-bracket_` (private)** — a string representing the opening bracket
  of the range formatted result (the default string value is `"["`)
  (exposition-only member object*)
- **`closing-bracket_` (private)** — a string representing the closing bracket
  of the range formatted result (the default string value is `"]"`)
  (exposition-only member object*)

### Member functions

- **set_separator** — sets a specified separator for the range formatted result
  (public member function)
- **set_brackets** — sets a specified opening and closing brackets for the range
  formatted result (public member function)
- **underlying** — returns the underlying formatter (public member function)
- **parse** — parses the format specifier as specified by range-format-spec
  (public member function)
- **format** — writes the range formatted output as specified by
  range-format-spec (public member function)

## std::range_formatter::set_separator

```cpp
constexpr void set_separator( std::basic_string_view<CharT> sep ) noexcept;
```

Assigns `sep` to `separator_`.

## std::range_formatter::set_brackets

```cpp
constexpr void set_brackets( std::basic_string_view<CharT> opening,
                             std::basic_string_view<CharT> closing ) noexcept;
```

Assigns `opening` and `closing` to `opening-bracket_` and `closing-bracket_`,
respectively.

## std::range_formatter::underlying

```cpp
constexpr std::formatter<T, CharT>& underlying();  // (1)
constexpr const std::formatter<T, CharT>& underlying() const;  // (2)
```

Returns `underlying_`, the underlying formatter.

## std::range_formatter::parse

```cpp
template< class ParseContext >
constexpr auto parse( ParseContext& ctx ) -> typename ParseContext::iterator;
```

Parses the format specifiers as a range-format-spec and stores the parsed
specifiers in the current object.

Calls `underlying_.parse(ctx)` to parse format-spec in range-format-spec or, if
the latter is not present, an empty format-spec.

If range-type or the `n` option is present, the values of `opening-bracket`,
`closing-bracket`, and `separator` are modified as required.

It calls `underlying_.set_debug_format()` if:

- the range-type is neither `s` nor `?s`,
- `underlying_.set_debug_format()` is a valid expression, and
- there is no range-underlying-spec.

Returns an iterator past the end of the range-format-spec.

## std::range_formatter::format

```cpp
template< ranges::input_range R, class FormatContext >
  requires std::formattable<ranges::range_reference_t<R>, CharT> &&
           std::same_as<std::remove_cvref_t<ranges::range_reference_t<R>>, T>
auto format( R&& r, FormatContext& ctx ) const -> typename FormatContext::iterator;
```

If the range-type was either `s` or `?s`, it writes the formatted
`std::basic_string<CharT>(std::from_range, r)` as a string or an escaped string,
respectively, into `ctx.out()`.

Otherwise, it writes the following into `ctx.out()` as specified by
range-format-spec, in order:

- `opening-bracket_`,
- for each formattable element `e` of the range `r`:
- `closing-bracket_`.

Returns an iterator past the end of the output range.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3892 | C++23 | incorrect formatting of nested ranges | corrected

### See also

- **formatter (C++20)** — defines formatting rules for a given type (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/utility/format/range_formatter*
