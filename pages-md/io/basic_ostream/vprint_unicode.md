# vprint_unicode(std::ostream)

```cpp
void vprint_unicode( std::ostream& os, std::string_view fmt, std::format_args args );  // (since C++23)
```

Behaves as FormattedOutputFunction (except that some details of error reporting
differ):

- First, the function constructs and checks the sentry object.
- Then, initializes an automatic variable as if by `std::string out =
  std::vformat(os.getloc(), fmt, args);`.
- Finally, inserts the character sequence `[out.begin(), out.end())` into `os`,
  that is, if `os` refers to a terminal capable of displaying Unicode, writes
  `out` to this terminal using the native Unicode API. Otherwise, writes
  unmodified `out` to the `os`.

### Parameters

- **os** — output stream to insert data into
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
- Propagate any exception thrown by any formatter, e.g. `std::format_error`,
  without regard to the value of `os.exceptions()` and without turning on
  `ios_base::badbit` in the error state of `os`.
- May throw `ios_base::failure` caused by `os.setstate(ios_base::badbit)` which
  is called if an insertion into `os` fails.

### Notes

If invoking the native Unicode API requires transcoding, the invalid code units
are substituted with `U+FFFD` REPLACEMENT CHARACTER (see "The Unicode Standard
Version 14.0 - Core Specification", Chapter 3.9).

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_print` | 202207L | (C++23) | Formatted output
  `__cpp_lib_format` | 202207L | (C++23) | Exposing `std::basic_format_string`

### Example

### See also

- **vprint_nonunicode(std::ostream) (C++23)** — outputs character data using
  type-erased argument representation (function)
- **print(std::ostream) (C++23)** — outputs formatted representation of the
  arguments (function template)
- **operator<<(std::basic_ostream)** — inserts character data or insert into
  rvalue stream (function template)
- **vprint_unicode (C++23)** — prints to Unicode capable `stdout` or a file
  stream using type-erased argument representation (function)
- **format (C++20)** — stores formatted representation of the arguments in a new
  string (function template)

### External links

  1. | Unicode
  2. | The Unicode Standard Version 14.0 - Core Specification

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ostream/vprint_unicode*
