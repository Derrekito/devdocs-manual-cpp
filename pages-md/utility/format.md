# Formatting library (since C++20)

The text formatting library offers a safe and extensible alternative to the
printf family of functions. It is intended to complement the existing C++ I/O
streams library.

### Formatting functions

- **format (C++20)** — stores formatted representation of the arguments in a new
  string (function template)
- **format_to (C++20)** — writes out formatted representation of its arguments
  through an output iterator (function template)
- **format_to_n (C++20)** — writes out formatted representation of its arguments
  through an output iterator, not exceeding specified size (function template)
- **formatted_size (C++20)** — determines the number of characters necessary to
  store the formatted representation of its arguments (function template)

### Format strings

- **basic_format_stringformat_stringwformat_string (C++20)(C++20)(C++20)** —
  class template that performs compile-time format string checks at construction
  time (class template)
- **runtime_format (C++26)** — creates runtime format strings directly usable in
  user-oriented formatting functions (function)

### Formatting concepts

- **formattable (C++23)** — specifies that a type is formattable, that is, it
  specializes `std::formatter` and provides member functions `parse` and
  `format` (concept)

### Extensibility support and implementation detail

- **vformat (C++20)** — non-template variant of `std::format` using type-erased
  argument representation (function)
- **vformat_to (C++20)** — non-template variant of `std::format_to` using
  type-erased argument representation (function template)
- **make_format_argsmake_wformat_args (C++20)(C++20)** — creates a type-erased
  object referencing all formatting arguments, convertible to `format_args`
  (function template)
- **visit_format_arg (C++20) (deprecated in C++26)** — argument visitation
  interface for user-defined formatters (function template)
- **formatter (C++20)** — defines formatting rules for a given type (class
  template)
- **range_formatter (C++23)** — class template that helps implementing
  `std::formatter` specializations for range types (class template)
- **range_format (C++23)** — specifies how a range should be formatted (enum)
- **format_kind (C++23)** — selects a suited `std::range_format` for a range
  (variable template)
- **basic_format_arg (C++20)** — class template that provides access to a
  formatting argument for user-defined formatters (class template)
- **basic_format_argsformat_argswformat_args (C++20)(C++20)(C++20)** — class
  that provides access to all formatting arguments (class template)
- **basic_format_contextformat_contextwformat_context (C++20)(C++20)(C++20)** —
  formatting state, including all formatting arguments and the output iterator
  (class template)
- **basic_format_parse_contextformat_parse_contextwformat_parse_context
  (C++20)(C++20)(C++20)** — formatting string parser state (class template)
- **format_error (C++20)** — exception type thrown on formatting errors (class)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_format` | 201907L | (C++20) | Text formatting
  202106L | (C++20) (DR) | Compile-time format string checks; Reducing
      parameterization of `std::vformat_to`
  202110L | (C++20) (DR) | Fixing locale handling in chrono formatters;
      Supporting non-const-formattable types
  202207L | (C++23) | Exposing `std::basic_format_string`; Clarify handling of
      encodings in localized formatting of chrono types
  202304L | (C++26) | Formatting pointers
  202305L | (C++26) | Type-checking format args
  202306L | (C++26) | Member `std::basic_format_arg::visit`
  `__cpp_lib_format_ranges` | 202207L | (C++23) | Formatting ranges

We intentionally treat the addition of `std::basic_format_string` (P2508) as a
defect report because all known implementations make these components available
in C++20 mode, although it is not so categorized officially.

### Example

```cpp
#include <cassert>
#include <format>

int main()
{
    std::string message = std::format("The answer is {}.", 42);
    assert(message == "The answer is 42.");
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2418R2 | C++20 | objects that are neither const-formattable nor copyable
      (such as generator-like objects) are not formattable | allow formatting
      these objects (relaxed formatter requirements)
  P2508R1 | C++20 | there's no user-visible name for this facility | the name
      `basic_format_string` is exposed

### See also

- **print (C++23)** — prints to `stdout` or a file stream using **formatted**
  representation of the arguments (function template)
- **println (C++23)** — same as `std::print` except that each print is
  terminated by additional new line (function template)
- **print(std::ostream) (C++23)** — outputs **formatted** representation of the
  arguments (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/format*
