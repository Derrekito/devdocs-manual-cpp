# std::*range-default-formatter*<std::range_format::sequence>

```cpp
template< ranges::input_range R, class CharT >
struct range-default-formatter<range_format::sequence, R, CharT>;  // (since C++23) (exposition only*)
```

The class template `range-default-formatter` for range types is specialized for
formatting range as a sequence of elements if `std::format_kind<R>` is
`std::range_format::sequence`.

### Member types

- **`maybe-const-r` (private)** — /*fmt-maybe-const*/<R, CharT> (exposition-only
  member type*)

### Member objects

- **`underlying_` (private)** — the underlying formatter of type
  `std::range_formatter<std::remove_cvref_t<ranges::range_reference_t<maybe-const-r>>,
  CharT>` (exposition-only member object*)

### Member functions

- **(constructor) (implicitly declared)** — constructs a
  `range-default-formatter` (public member function)
- **set_separator** — sets a specified separator for the range formatted result
  (public member function)
- **set_brackets** — sets a specified opening and closing brackets for the range
  formatted result (public member function)
- **parse** — parses the format specifier as specified by range-format-spec
  (public member function)
- **format** — writes the range formatted output as specified by
  range-format-spec (public member function)

## std::*range-default-formatter*<std::range_format::sequence>::set_separator

```cpp
constexpr void set_separator( std::basic_string_view<CharT> sep ) noexcept;
```

Equivalent to a call to `underlying_.set_separator(sep)`.

## std::*range-default-formatter*<std::range_format::sequence>::set_brackets

```cpp
constexpr void set_brackets( std::basic_string_view<CharT> opening,
                             std::basic_string_view<CharT> closing ) noexcept;
```

Equivalent to a call to `underlying_.set_brackets(opening, closing)`.

## std::*range-default-formatter*<std::range_format::sequence>::parse

```cpp
template< class ParseContext >
constexpr auto parse( ParseContext& ctx ) -> typename ParseContext::iterator;
```

Equivalent to `return underlying_.parse(ctx);`.

Returns an iterator past the end of the range-format-spec.

## std::*range-default-formatter*<std::range_format::sequence>::format

```cpp
template< class FormatContext >
auto format( maybe-const-r& elems, FormatContext& ctx ) const
    -> typename FormatContext::iterator;
```

Equivalent to `return underlying_.format(elems, ctx);`.

Returns an iterator past the end of the output range.

### See also

- **formatter (C++20)** — defines formatting rules for a given type (class
  template)
- **range_formatter (C++23)** — class template that helps implementing
  `std::formatter` specializations for range types (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/format/ranges_formatter/range_default_formatter_sequence*
