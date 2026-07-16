# std::formatted_size

```cpp
template< class... Args >
std::size_t formatted_size( std::format_string<Args...> fmt, Args&&... args );  // (1) (since C++20)
template< class... Args >
std::size_t formatted_size( std::wformat_string<Args...> fmt, Args&&... args );  // (2) (since C++20)
template< class... Args >
std::size_t formatted_size( const std::locale& loc,
                            std::format_string<Args...> fmt, Args&&... args );  // (3) (since C++20)
template< class... Args >
std::size_t formatted_size( const std::locale& loc,
                            std::wformat_string<Args...> fmt, Args&&... args );  // (4) (since C++20)
```

Determine the total number of characters in the formatted string by formatting
`args` according to the format string `fmt`. If present, `loc` is used for
locale-specific formatting.

The behavior is undefined if `std::formatter<std::remove_cvref_t<Ti>, CharT>`
does not meet the BasicFormatter requirements for any `Ti` in `Args`.

### Parameters

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
- **loc** — `std::locale` used for locale-specific formatting

### Return value

The total number of characters in the formatted string.

### Exceptions

Propagates any exception thrown by formatter.

### Example

```cpp
#include <format>
#include <iostream>
#include <vector>
#include <string_view>

int main()
{
    using namespace std::literals::string_view_literals;

    constexpr auto fmt_str { "Hubble's H{0} {1} {2:*^4} miles/sec/mpc."sv };
    constexpr auto sub_zero { "₀"sv };  // { "\u2080"sv } => { 0xe2, 0x82, 0x80 };
    constexpr auto aprox_equ { "≅"sv }; // { "\u2245"sv } => { 0xe2, 0x89, 0x85 };
    constexpr int Ho { 42 }; // H₀

    const auto min_buffer_size = std::formatted_size(fmt_str, sub_zero, aprox_equ, Ho);

    std::cout << "Min buffer size = " << min_buffer_size << '\n';

    // Use std::vector as dynamic buffer. Note: buffer does not include the trailing '\0'.
    std::vector<char> buffer(min_buffer_size);

    std::format_to_n(buffer.data(), buffer.size(), fmt_str, sub_zero, aprox_equ, Ho);

    std::cout << "Buffer: \"" << std::string_view{buffer.data(), min_buffer_size} << "\"\n";

    // Or we can print the buffer directly by adding the trailing '\0'.
    buffer.push_back('\0');
    std::cout << "Buffer: \"" << buffer.data() << "\"\n";
}
```

Output:

```text
Min buffer size = 37
Buffer: "Hubble's H₀ ≅ *42* miles/sec/mpc."
Buffer: "Hubble's H₀ ≅ *42* miles/sec/mpc."
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2216R3 | C++20 | throws `std::format_error` for invalid format string |
      invalid format string results in compile-time error
  P2418R2 | C++20 | objects that are neither const-usable nor copyable (such as
      generator-like objects) are not formattable | allow formatting these
      objects
  P2508R1 | C++20 | there's no user-visible name for this facility | the name
      `basic_format_string` is exposed

### See also

- **format_to (C++20)** — writes out formatted representation of its arguments
  through an output iterator (function template)
- **format_to_n (C++20)** — writes out formatted representation of its arguments
  through an output iterator, not exceeding specified size (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/format/formatted_size*
