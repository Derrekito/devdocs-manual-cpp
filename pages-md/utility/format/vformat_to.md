# std::vformat_to

```cpp
template< class OutputIt >
OutputIt vformat_to( OutputIt out, std::string_view fmt, std::format_args args );  // (1) (since C++20)
template< class OutputIt >
OutputIt vformat_to( OutputIt out, std::wstring_view fmt, std::wformat_args args );  // (2) (since C++20)
template< class OutputIt >
OutputIt vformat_to( OutputIt out, const std::locale& loc,
                     std::string_view fmt, std::format_args args );  // (3) (since C++20)
template< class OutputIt >
OutputIt vformat_to( OutputIt out, const std::locale& loc,
                     std::wstring_view fmt, std::wformat_args args );  // (4) (since C++20)
```

Format arguments held by `args` according to the format string `fmt`, and write
the result to the output iterator `out`. If present, `loc` is used for
locale-specific formatting.

Let `CharT` be `decltype(fmt)::char_type` (`char` for overloads (1,3), `wchar_t`
for overloads (2,4)).

These overloads participate in overload resolution only if `OutputIt` satisfies
the concept `std::output_iterator<const CharT&>`.

The behavior is undefined if `OutputIt` does not model (meet the semantic
requirements of) the concept `std::output_iterator<const CharT&>`, or if
`std::formatter<Ti, CharT>` does not meet the Formatter requirements for any
`Ti` in the type of arguments.

### Parameters

- **out** — iterator to the output buffer
- **fmt** — an object that represents the format string. The format string
  consists of ordinary characters (except `{` and `}`), which are copied
  unchanged to the output, escape sequences `{{` and `}}`, which are replaced
  with `{` and `}` respectively in the output, and replacement fields. Each
  replacement field has the following format: `{` arg-id (optional) `}` (1) `{`
  arg-id (optional) `:` format-spec `}` (2) 1) replacement field without a
  format specification 2) replacement field with a format specification arg-id -
  specifies the index of the argument in `args` whose value is to be used for
  formatting; if it is omitted, the arguments are used in order. The arg-ids in
  a format string must all be present or all be omitted. Mixing manual and
  automatic indexing is an error. format-spec - the format specification defined
  by the `std::formatter` specialization for the corresponding argument. For
  basic types and standard string types, the format specification is interpreted
  as standard format specification. For chrono types, the format specification
  is interpreted as chrono format specification. For range types, the format
  specification is interpreted as range format specification. For `std::pair`
  and `std::tuple`, the format specification is interpreted as tuple format
  specification. For `std::thread::id` and `std::stacktrace_entry`, see thread
  id format specification and stacktrace entry format specification. For
  `std::basic_stacktrace`, no format specifier is allowed. (since C++23) For
  other formattable types, the format specification is determined by
  user-defined `formatter` specializations.
- **`{` arg-id (optional) `}`**
- **`{` arg-id (optional) `:` format-spec `}`**
- **arg-id** — specifies the index of the argument in `args` whose value is to
  be used for formatting; if it is omitted, the arguments are used in order. The
  arg-ids in a format string must all be present or all be omitted. Mixing
  manual and automatic indexing is an error.
- **format-spec** — the format specification defined by the `std::formatter`
  specialization for the corresponding argument.
- **For range types, the format specification is interpreted as range format
  specification. For `std::pair` and `std::tuple`, the format specification is
  interpreted as tuple format specification. For `std::thread::id` and
  `std::stacktrace_entry`, see thread id format specification and stacktrace
  entry format specification. For `std::basic_stacktrace`, no format specifier
  is allowed.** — (since C++23)
- **args** — arguments to be formatted
- **loc** — `std::locale` used for locale-specific formatting

### Return value

Iterator past the end of the output range.

### Exceptions

Throws `std::format_error` if `fmt` is not a valid format string for the
provided arguments. Also propagates any exception thrown by formatter or
iterator operations.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2216R3 | C++20 | type of `args` is parameterized on `OutputIt` | not
      parameterized

### See also

---
*Source: https://en.cppreference.com/w/cpp/utility/format/vformat_to*
