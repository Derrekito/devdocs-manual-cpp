# std::basic_format_string, std::format_string, std::wformat_string

```cpp
template< class CharT, class... Args >
struct basic_format_string;  // (1) (since C++20)
template< class... Args >
using format_string = basic_format_string<char, std::type_identity_t<Args>...>;  // (2) (since C++20)
template< class... Args >
using wformat_string = basic_format_string<wchar_t, std::type_identity_t<Args>...>;  // (3) (since C++20)
```

Class template `std::basic_format_string` wraps a `std::basic_string_view` that
will be used by formatting functions.

It performs compile-time format string checks at construction time only if the
constructor argument is not returned by `std::runtime_format`(since C++26).

### Member functions

- **(constructor)** — constructs a `basic_format_string`, raising compile error
  if the argument is not a format string (public member function)
- **get** — returns the wrapped string (public member function)

## std::basic_format_string::basic_format_string

```cpp
template< class T >
consteval basic_format_string( const T& s );  // (1)
basic_format_string( /*runtime-format-string*/<CharT> s ) noexcept;  // (2) (since C++26)
```

1) Constructs a `basic_format_string` object that stores a view of string `s`.
   If the argument is not a compile-time constant, or if it cannot be parsed as
   a format string for the formatting argument types `Args`, the construction is
   ill-formed.

- This overload participates in overload resolution only if `const T&` models
  `std::convertible_to<std::basic_string_view<CharT>>`.

2) Constructs a `basic_format_string` object that stores a view of string `s` as
   returned by `std::runtime_format`. It does not perform format string checks
   upon construction.

### Parameters

- **s** — an object that represents the format string. The format string
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

## std::basic_format_string::get

```cpp
constexpr std::basic_string_view<CharT> get() const noexcept;
```

Returns the stored string view.

### Notes

The alias templates `format_string` and `wformat_string` use
`std::type_identity_t` to inhibit template argument deduction. Typically, when
they appear as a function parameter, their template arguments are deduced from
other function arguments.

```cpp
template< class... Args >
std::string format( std::format_string<Args...> fmt, Args&&... args );

auto s = format("{} {}", 1.0, 2);
// Calls format<double, int>. Args are deduced from 1.0, 2
// Due to the use of type_identity_t in format_string, template argument deduction
// does not consider the type of the format string.
```

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2508R1 | C++20 | there's no user-visible name for this facility | the name
      `basic_format_string` is exposed

---
*Source: https://en.cppreference.com/w/cpp/utility/format/basic_format_string*
