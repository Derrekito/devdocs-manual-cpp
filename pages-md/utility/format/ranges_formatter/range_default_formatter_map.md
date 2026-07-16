# std::*range-default-formatter*<std::range_format::map>

```cpp
template< ranges::input_range R, class CharT >
struct range-default-formatter<range_format::map, R, CharT>;  // (since C++23) (exposition only*)
```

The class template `range-default-formatter` for range types is specialized for
formatting range as a map of keys to values if `std::format_kind<R>` is
`std::range_format::map`.

### Member types

- **`maybe-const-map` (private)** — /*fmt-maybe-const*/<R, CharT>
  (exposition-only member type*)
- **`element-type` (private)** —
  `std::remove_cvref_t<ranges::range_reference_t<maybe-const-map>>`

### Member objects

- **`underlying_` (private)** — the underlying formatter of type
  `std::range_formatter<element-type, CharT>` (exposition-only member object*)

### Member functions

- **(constructor)** — constructs a `range-default-formatter` (public member
  function)
- **parse** — parses the format specifier as specified by range-format-spec
  (public member function)
- **format** — writes the range formatted output as specified by
  range-format-spec (public member function)

## std::*range-default-formatter*<std::range_format::map>::*range-default-formatter*

```cpp
constexpr range-default-formatter();
```

Equivalent to: `underlying_.set_brackets(STATICALLY_WIDEN<CharT>("{"),
STATICALLY_WIDEN<CharT>("}"));underlying_.underlying().set_brackets({},
{});underlying_.underlying().set_separator(STATICALLY-WIDEN<charT>(": "));`

where `STATICALLY_WIDEN<CharT>("...")` is `"..."` if `CharT` is `char`, and
`L"..."` if `CharT` is `wchar_t`.

The program is ill-formed unless:

- `element-type` is a specialization of `std::pair`, or
- `element-type` is a specialization of `std::tuple` and
  `std::tuple_size_v<element-type>` is `2`.

## std::*range-default-formatter*<std::range_format::map>::parse

```cpp
template< class ParseContext >
constexpr auto parse( ParseContext& ctx ) -> typename ParseContext::iterator;
```

Equivalent to `return underlying_.parse(ctx);`.

Returns an iterator past the end of the range-format-spec.

## std::*range-default-formatter*<std::range_format::map>::format

```cpp
template< class FormatContext >
auto format( maybe-const-map& r, FormatContext& ctx ) const
    -> typename FormatContext::iterator;
```

Equivalent to `return underlying_.format(r, ctx);`.

Returns an iterator past the end of the output range.

### See also

- **formatter (C++20)** — defines formatting rules for a given type (class
  template)
- **range_formatter (C++23)** — class template that helps implementing
  `std::formatter` specializations for range types (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/format/ranges_formatter/range_default_formatter_map*
