# std::vprint_unicode

```cpp
void vprint_unicode( std::FILE* stream,
                     std::string_view fmt, std::format_args args );  // (1) (since C++23)
void vprint_unicode( std::string_view fmt, std::format_args args );  // (2) (since C++23)
```

Format `args` according to the format string `fmt`, and writes the result to the
stream.

Let `out` be a temporary `string` produced by formatting the arguments in `args`
with `std::vformat(fmt, args)`.

1) If `stream` refers to a terminal capable of displaying Unicode, writes `out`
   to this terminal using the native Unicode API. Otherwise, writes unmodified
   `out` to the `stream`.

2) same as (1) when `stream` is equal to the standard C output stream `stdout`,
   i.e. `std::vprint_unicode(stdout, fmt, args);`

The behavior is undefined if:

- `stream` is not a valid pointer to a C stream;
- `out` contains invalid Unicode code units.

### Parameters

- **stream** — output file stream to write to
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

### Return value

(none)

### Exceptions

- `std::bad_alloc` on allocation failure.
- `std::system_error`, if writing to the stream fails.
- Propagates any exception thrown by used formatters, e.g. `std::format_error`.

### Notes

The C++ standard encourages the implementers to produce a diagnostic message if
`out` contains invalid Unicode code units.

On POSIX, the stream refers to a terminal if the expression
`isatty(fileno(stream)) != 0` holds true (see POSIX documentation for `isatty`,
and `fileno`).

On Windows, the stream refers to a terminal if the expression
`GetConsoleMode(_get_osfhandle(_fileno(stream)))` returns nonzero (see Windows
documentation for `GetConsoleMode`, `_get_osfhandle`, and `_fileno`). The native
Unicode API on Windows is `WriteConsoleW`.

If invoking the native Unicode API requires transcoding, the invalid code units
are substituted with `U+FFFD` REPLACEMENT CHARACTER (see "The Unicode Standard
Version 14.0 - Core Specification", Chapter 3.9).

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_print` | 202207L | (C++23) | Formatted output
  `__cpp_lib_format` | 202207L | (C++23) | Exposing `std::basic_format_string`

### Example

### See also

- **vprint_nonunicode (C++23)** — prints to `stdout` or a file stream using
  type-erased argument representation (function)
- **vprint_unicode(std::ostream) (C++23)** — performs Unicode aware output using
  type-erased argument representation (function)
- **print (C++23)** — prints to `stdout` or a file stream using formatted
  representation of the arguments (function template)
- **format (C++20)** — stores formatted representation of the arguments in a new
  string (function template)

### External links

  1. | Unicode
  2. | The Unicode Standard Version 14.0 - Core Specification

---
*Source: https://en.cppreference.com/w/cpp/io/vprint_unicode*
