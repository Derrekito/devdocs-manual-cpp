# std::runtime_format

```cpp
/*runtime-format-string*/<char> runtime_format( std::string_view fmt ) noexcept;  // (1) (since C++26)
/*runtime-format-string*/<wchar_t> runtime_format( std::wstring_view fmt ) noexcept;  // (2) (since C++26)
```

Returns an object that stores a runtime format string directly usable in
user-oriented formatting functions and can be implicitly converted to
`std::basic_format_string`.

1,2) Equivalent to `return fmt;`.

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

### Return value

An object holding the runtime format string of the exposition-only type:

## **Class template** `runtime-format-string` `<CharT>`

```cpp
template< class CharT >
struct /*runtime-format-string*/;  // (exposition only*)
```

#### Member objects

The returned object contains an exposition-only non-static data member `str` of
type `std::basic_string_view<CharT>`.

#### Constructors and assignments

```cpp
/*runtime-format-string*/( std::basic_string_view<CharT> s ) noexcept;  // (1)
/*runtime-format-string*/( const /*runtime-format-string*/& ) = delete;  // (2)
/*runtime-format-string*/& operator=( const /*runtime-format-string*/& ) = delete;  // (3)
```

1) Initializes `str` with `s`.

2) Copy constructor is explicitly deleted. The type is neither copyable nor
   movable.

3) The assignment is explicitly deleted.

### Notes

Since the return type of `runtime_format` is neither copyable nor movable, an
attempt of passing `runtime_fmt` as glvalue inhibits the construction of
`std::basic_format_string` which results in program ill-formed. To construct
`std::basic_format_string` with `runtime_format`, the returned value of
`runtime_format` is passed directly on `std::basic_format_string` as prvalue
where copy elision is guaranteed.

```cpp
auto runtime_fmt = std::runtime_format("{}");

auto s0 = std::format(runtime_fmt, 1); // error
auto s1 = std::format(std::move(runtime_fmt), 1); // still error
auto s2 = std::format(std::runtime_format("{}"), 1); // ok
```

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_format` | 202311L | (C++26) | Runtime format strings

### Example

```cpp
#include <format>
#include <print>
#include <string>
#include <string_view>

int main()
{
    std::print("Hello {}!\n", "world");

    std::string fmt;
    for (int i{}; i != 3; ++i)
    {
        fmt += "{} "; // constructs the formatting string
        std::print("{} : ", fmt);
        std::println(std::runtime_format(fmt), "alpha", 'Z', 3.14, "unused");
    }
}
```

Output:

```text
Hello world!
{}  : alpha
{} {}  : alpha Z
{} {} {}  : alpha Z 3.14
```

### See also

- **format (C++20)** — stores formatted representation of the arguments in a new
  string (function template)
- **vformat (C++20)** — non-template variant of `std::format` using type-erased
  argument representation (function)
- **basic_format_stringformat_stringwformat_string (C++20)(C++20)(C++20)** —
  class template that performs compile-time format string checks at construction
  time (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/format/runtime_format*
