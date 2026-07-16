# Formatting library (since C++20)

`<format>` is a type-safe, extensible alternative to the `printf`
family: instead of `%d`/`%s`-style specifiers that the compiler cannot
check against your arguments, you write `{}`-style placeholders and
the library dispatches on each argument's actual type. It complements,
rather than replaces, the iostreams library.

```cpp skip
std::format("{} is {}", name, age);          // build a new std::string
std::format_to(out_it, "{}", x);              // write through an output iterator
std::format_to_n(out_it, n, "{}", x);         // like format_to, capped at n chars
std::formatted_size("{}", x);                 // how many characters it would take
```

All since C++20.

### Formatting functions

| Function | What it does |
| --- | --- |
| `format` | returns a new `std::string` holding the formatted result |
| `format_to` | writes the formatted result through an output iterator |
| `format_to_n` | like `format_to`, but stops after at most `n` characters |
| `formatted_size` | computes the length the formatted result would have |

### Guarantees and costs

- The format string is checked at **compile time** when it is a
  string literal (or otherwise a constant expression): a placeholder
  that doesn't match its argument's type, or an argument count
  mismatch, is a compile error, not a runtime surprise. Use
  `std::runtime_format` (C++26) when the format string is genuinely
  only known at runtime.
- Extensible: any type can opt in by specializing `std::formatter` and
  implementing `parse`/`format`; ranges get this via
  `std::range_formatter`.
- `vformat`/`vformat_to` are the type-erased primitives that `format`
  and friends are built on — reach for them only when writing
  formatting infrastructure of your own.

### Gotchas

- A malformed format string (unbalanced braces, an unknown
  placeholder) that isn't caught at compile time throws
  `std::format_error` at runtime — this is the main exception path in
  this library.
- Compile-time checking only fires when the format string is a
  constant expression; a `std::string` built at runtime and passed as
  the format argument bypasses it, pushing the check to runtime via
  `format_error`.

### Example

```cpp c++20
#include <cassert>
#include <format>

int main()
{
    std::string message = std::format("The answer is {}.", 42);
    assert(message == "The answer is 42.");
}
```

### Reference

Related components, condensed:

- **formatter** — class template defining how a type formats; specialize
  it to make a type formattable
- **formattable (C++23)** — concept satisfied by types with a working `formatter`
- **basic_format_string, format_string, wformat_string** — perform the
  compile-time format-string check at construction
- **runtime_format (C++26)** — opts a runtime-known string into the
  formatting functions, bypassing the compile-time check
- **basic_format_args, basic_format_arg, basic_format_context,
  basic_format_parse_context** — type-erased argument/state plumbing
  used by `vformat`-family functions and custom formatters
- **format_error** — exception thrown on a formatting error
- **range_formatter, range_format, format_kind (C++23)** — support for
  formatting range types

Feature-test macro: `__cpp_lib_format` — `201907L` (C++20, the
library), `202106L` (C++20 DR, compile-time format-string checks),
`202110L` (C++20 DR, chrono-formatter locale fixes), `202207L` (C++23,
exposing `basic_format_string`); `__cpp_lib_format_ranges` — `202207L`
(C++23, formatting ranges).

### See also

- **print (C++23)** — formats and prints directly to `stdout` or a file stream
- **println (C++23)** — like `print`, plus a trailing newline
- **to_string (C++11)** — simpler numeric-only conversion, no format string

---
*Source: https://en.cppreference.com/w/cpp/utility/format*
