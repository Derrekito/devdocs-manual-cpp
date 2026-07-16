# std::println

```cpp
template< class... Args >
void println( std::FILE* stream,
              std::format_string<Args...> fmt, Args&&... args );  // (1) (since C++23)
template< class... Args >
void println( std::format_string<Args...> fmt, Args&&... args );  // (2) (since C++23)
```

Format `args` according to the format string `fmt` with appended `'\n'` (which
means that each output ends with a new-line), and print the result to a stream.

1) Equivalent to: `std::print(stream, "{}\n", std::format(fmt, args...));`

2) same as (1) when `stream` is equal to the standard C output stream `stdout`,
   i.e. `std::println(stdout, fmt, args...);`

The behavior is undefined if `std::formatter<Ti, char>` does not meet the
BasicFormatter requirements for any `Ti` in `Args` (as required by
`std::make_format_args`).

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
- **args...** — arguments to be formatted

### Return value

(none)

### Exceptions

- `std::bad_alloc` on allocation failure.
- `std::system_error`, if writing to the stream fails.
- Propagates any exception thrown by used formatters, e.g. `std::format_error`.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_print` | 202207L | (C++23) | Formatted output
  `__cpp_lib_format` | 202207L | (C++23) | Exposing `std::basic_format_string`

### Example

```cpp
#include <print>

int main()
{
    // Each call to std::println ends with new-line
    std::println("Please");
    std::println("enter");

    std::print("pass");
    std::print("word");

    std::println(""); // same effect as std::print("\n");
}
```

Output:

```text
Please
enter
password
```

### See also

- **print (C++23)** — prints to `stdout` or a file stream using formatted
  representation of the arguments (function template)
- **println(std::ostream) (C++23)** — outputs formatted representation of the
  arguments with appended `'\n'` (function template)
- **format (C++20)** — stores formatted representation of the arguments in a new
  string (function template)
- **printffprintfsprintfsnprintf (C++11)** — prints formatted output to
  `stdout`, a file stream or a buffer (function)

---
*Source: https://en.cppreference.com/w/cpp/io/println*
