# C++ named requirements: Formatter (since C++20)

**Formatter** is a type that abstracts formatting operations for a given
formatting argument type and character type. Specializations of `std::formatter`
provided by the standard library are required to meet the requirements of
Formatter except as noted otherwise.

A **Formatter** is able to format both const and non-const arguments, typically
by providing a `format` member function that takes a const reference.

### Requirements

A type satisfies Formatter if it satisfies BasicFormatter and given

- `Arg`, a formatting argument type
- `CharT`, a character type
- `Formatter`, a Formatter type for types `Arg` and `CharT`
- `OutputIt`, a LegacyOutputIterator type
- `f`, a value of type (possibly const) `Formatter`
- `g`, a value of type `Formatter`
- `arg`, an lvalue of type `Arg`
- `t`, a value of type convertible to (possibly const) `Arg`
- `ParseContext`, an alias of `std::basic_format_parse_context<CharT>`
- `FormatContext`, an alias of `std::basic_format_context<OutputIt, CharT>`
- `parse_ctx`, an lvalue of type `ParseContext`
- `format_ctx`, an lvalue of type `FormatContext`

  Expression | Return type | Semantics
  `f.format(t, format_ctx)` | `FormatContext::iterator` | Formats `t` according
      to the specifiers stored in `f`, writes the output to `format_ctx.out()`
      and returns an end iterator of the output range. The output shall only
      depend on `t`, `format_ctx.locale()`, the range `[parse_ctx.begin(),
      parse_ctx.end())` from the last call to `f.parse(parse_ctx)`, and
      `format_ctx.arg(n)` for any value `n` of type `std::size_t`.
  `f.format(arg, format_ctx)` | `FormatContext::iterator` | As above, but does
      not modify `arg`.

---
*Source: https://en.cppreference.com/w/cpp/named_req/Formatter*
