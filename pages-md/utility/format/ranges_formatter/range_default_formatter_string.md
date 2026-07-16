# std::*range-default-formatter*<std::range_format::string>, std::*range-default-formatter*<std::range_format::debug_string>

```cpp
template< std::range_format K, ranges::input_range R, class CharT >
    requires (K == std::range_format::string || K == std::range_format::debug_string)
struct range-default-formatter<K, R, CharT>;  // (since C++23) (exposition only*)
```

The class template `range-default-formatter` for range types is specialized for
formatting range as a string or an escaped string if `std::format_kind<R>` is
either `std::range_format::string` or `std::range_format::debug_string`.

### Member objects

- **`underlying_` (private)** — the underlying formatter of type
  `std::formatter<std::basic_string<CharT>, CharT>` (exposition-only member
  object*)

### Member functions

- **(constructor) (implicitly declared)** — constructs a
  `range-default-formatter` (public member function)
- **parse** — parses the format specifier as specified by std-format-spec
  (public member function)
- **format** — writes the formatted output as specified by std-format-spec
  (public member function)

## std::*range-default-formatter*<std::range_format::string>::parse std::*range-default-formatter*<std::range_format::debug_string>::parse

```cpp
template< class ParseContext >
constexpr auto parse( ParseContext& ctx ) -> typename ParseContext::iterator;
```

Equivalent to: `auto i = underlying_.parse(ctx);if constexpr (K ==
std::range_format::debug_string) underlying_.set_debug_format();return i;`

Returns an iterator past the end of the std-format-spec.

## std::*range-default-formatter*<std::range_format::string>::format std::*range-default-formatter*<std::range_format::debug_string>::format

```cpp
template< class FormatContext >
auto format( /* see below */& r, FormatContext& ctx ) const
    -> typename FormatContext::iterator;
```

If `ranges::input_range<const R>` is `true`, the type of `r` is `const R&`.
Otherwise, the type is `R&`.

Let `s` be a `std::basic_string<CharT>` as if by constructing `s` with
`std::basic_string<CharT>(std::from_range, r)` such that `ranges::equal(s, r)`
is `true`.

Equivalent to `return underlying_.format(s, ctx);`.

Returns an iterator past the end of the output range.

### See also

- **formatter (C++20)** — defines formatting rules for a given type (class
  template)
- **range_formatter (C++23)** — class template that helps implementing
  `std::formatter` specializations for range types (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/format/ranges_formatter/range_default_formatter_string*
