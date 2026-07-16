# C++ named requirements: BasicFormatter (since C++20)

**BasicFormatter** is a type that abstracts formatting operations for a given
formatting argument type and character type. Specializations of `std::formatter`
are required to meet the requirements of BasicFormatter.

A BasicFormatter is a Formatter if it is able to format both const and non-const
arguments.

### Requirements

A type satisfies BasicFormatter if it is semiregular, meaning it satisfies:

- DefaultConstructible
- CopyConstructible
- CopyAssignable
- Destructible
- Swappable

And, given

- `Arg`, a formatting argument type
- `CharT`, a character type
- `BasicFormatter`, a BasicFormatter type for types `Arg` and `CharT`
- `OutputIt`, a LegacyOutputIterator type
- `f`, a value of type (possibly const) `BasicFormatter`
- `g`, a value of type `BasicFormatter`
- `arg`, an lvalue of type `Arg`
- `ParseContext`, an alias of `std::basic_format_parse_context<CharT>`
- `FormatContext`, an alias of `std::basic_format_context<OutputIt, CharT>`
- `parse_ctx`, an lvalue of type `ParseContext`
- `format_ctx`, an lvalue of type `FormatContext`

  Expression | Return type | Semantics
  `g.parse(parse_ctx)` | `ParseContext::iterator` | Parses the format-spec
      `[parse_ctx.begin(), parse_ctx.end())` for type `Arg` until the first
      unmatched character. Throws `std::format_error` unless the whole range is
      parsed or the unmatched character is `}`. [note 1] Stores the parsed
      format specifiers in `g` and returns an end iterator of the parsed range.
  `f.format(arg, format_ctx)` | `FormatContext::iterator` | Formats `arg`
      according to the specifiers stored in `f`, writes the output to
      `format_ctx.out()` and returns an end iterator of the output range. The
      output shall only depend on `arg`, `format_ctx.locale()`, the range
      `[parse_ctx.begin(), parse_ctx.end())` from the last call to
      `f.parse(parse_ctx)`, and `format_ctx.arg(n)` for any value `n` of type
      `std::size_t`.

1. This allows formatters to emit meaningful error messages.

---
*Source: https://en.cppreference.com/w/cpp/named_req/BasicFormatter*
