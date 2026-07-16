# std::formatter<*pair-or-tuple*>

```cpp
template< class CharT, std::formattable<CharT>... Ts >
struct formatter</*pair-or-tuple*/<Ts...>, CharT>;  // (since C++23)
```

The template specialization of `std::formatter` for the `std::pair` and
`std::tuple` allows users to convert a pair or a tuple to its textual
representation as a collection of elements using formatting functions.

The exposition-only name `/*pair-or-tuple*/` denotes either class template
`std::pair` or `std::tuple`.

### Format specification

The syntax of tuple-format-spec is:

```text
tuple-fill-and-align ﻿(optional) width ﻿(optional) tuple-type ﻿(optional)
```

The tuple-fill-and-align is interpreted the same way as a fill-and-align except
that the fill in tuple-fill-and-align is any character other than `{`, `}`, or
`:`.

The width is described in standard format width specification.

The tuple-type changes the way a tuple is formatted, with certain options only
valid with certain argument types.

The available tuple presentation types are:

- `m`: Indicates that both opening and closing brackets should be `""` while the
  separator should be `": "`.
- `n`: Indicates that separator, opening and closing brackets should be `""`.

### Member objects

- **`underlying_` (private)** — tuple of underlying formatters of type
  `std::tuple<std::formatter<std::remove_cvref_t<Ts>, CharT>...>`
  (exposition-only member object*)
- **`separator_` (private)** — a string representing the separator of the tuple
  formatted result (the default value is `", "`) (exposition-only member
  object*)
- **`opening-bracket_` (private)** — a string representing the opening bracket
  of the tuple formatted result (the default value is `"("`) (exposition-only
  member object*)
- **`closing-bracket_` (private)** — a string representing the closing bracket
  of the tuple formatted result (the default value is `")"`) (exposition-only
  member object*)

### Member functions

- **set_separator** — sets a specified separator for the tuple formatted result
  (public member function)
- **set_brackets** — sets a specified opening and closing brackets for the tuple
  formatted result (public member function)
- **parse** — parses the format specifier as specified by tuple-format-spec
  (public member function)
- **format** — writes the tuple formatted output as specified by
  tuple-format-spec (public member function)

## std::formatter<*pair-or-tuple*>::set_separator

```cpp
constexpr void set_separator( std::basic_string_view<CharT> sep ) noexcept;
```

Assigns `sep` to `separator_`.

## std::formatter<*pair-or-tuple*>::set_brackets

```cpp
constexpr void set_brackets( std::basic_string_view<CharT> opening,
                             std::basic_string_view<CharT> closing ) noexcept;
```

Assigns `opening` and `closing` to `opening-bracket_` and `closing-bracket_`,
respectively.

## std::formatter<*pair-or-tuple*>::parse

```cpp
template< class ParseContext >
constexpr auto parse( ParseContext& ctx ) -> typename ParseContext::iterator;
```

Parses the format specifiers as a tuple-format-spec and stores the parsed
specifiers in the current object.

If tuple-type or the `n` option is present, the values of `opening-bracket`,
`closing-bracket`, and `separator` are modified as required.

For each element `e` in `underlying_`, calls `e.parse(ctx)` to parse an empty
format-spec and, if `e.set_debug_format()` is a valid expression, calls
`e.set_debug_format()`.

Returns an iterator past the end of the tuple-format-spec.

## std::formatter<*pair-or-tuple*>::format

```cpp
template< class FormatContext >
auto format( /* see below */& elems, FormatContext& ctx ) const
    -> typename FormatContext::iterator;
```

If `(std::formattable<const Ts, CharT> && ...)` is `true`, then the type of
`elems` is `const /*pair-or-tuple*/<Ts...>&`. Otherwise, the type is
`/*pair-or-tuple*/<Ts...>&`.

Writes the following into `ctx.out()` as specified by tuple-format-spec, in
order:

- `opening-bracket_`,
- for each index `I` in half-open interval `[0, sizeof...(Ts))`:
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
*Source: https://en.cppreference.com/w/cpp/utility/format/tuple_formatter*
