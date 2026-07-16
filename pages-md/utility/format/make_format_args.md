# std::make_format_args, std::make_wformat_args

```cpp
template< class Context = std::format_context, class... Args >
/*format-arg-store*/<Context, Args...>
    make_format_args( Args&... args );  // (1) (since C++20)
template< class... Args >
/*format-arg-store*/<std::wformat_context, Args...>
    make_wformat_args( Args&... args );  // (2) (since C++20)
```

Returns an object that stores an array of formatting arguments and can be
implicitly converted to `std::basic_format_args<Context>`.

The behavior is undefined if `typename Context::template
formatter_type<std::remove_const_t<Ti>>` does not meet the BasicFormatter
requirements for any `Ti` in `Args`.

The program is ill-formed if for any type `Ti` in `Args`, `Ti` does not satisfy
`__formattable_with`<Context>.

2) Equivalent to `return std::make_format_args<std::wformat_context>(args...);`.

### Parameters

- **args...** — values to be used as formatting arguments

### Returns

An object that holds the formatting arguments.

For each argument `t` of type `T`, let `TD` be
`std::remove_const_t<std::remove_reference_t<T>>`. The corresponding
`std::basic_format_arg` in the result is determined as below:

- if `TD` is bool or `Context::char_type`, the `std::basic_format_arg` stores
  `t`;
- otherwise, if `TD` is char and `Context::char_type` is wchar_t, the
  `std::basic_format_arg` stores `static_cast<wchar_t>(static_cast<unsigned
  char>(t))`;
- otherwise, if `TD` is a signed integer type whose size is not greater than
  int, the `std::basic_format_arg` stores `static_cast<int>(t)`;
- otherwise, if `TD` is a unsigned integer type whose size is not greater than
  unsigned int, the `std::basic_format_arg` stores `static_cast<unsigned
  int>(t)`;
- otherwise, if `TD` is a signed integer type whose size is not greater than
  long long, the `std::basic_format_arg` stores `static_cast<long long>(t)`;
- otherwise, if `TD` is a unsigned integer type whose size is not greater than
  unsigned long long, the `std::basic_format_arg` stores `static_cast<unsigned
  long long>(t)`;
- otherwise, if `TD` is float, double, or long double, the
  `std::basic_format_arg` stores `t`;
- otherwise, if `TD` is a `std::basic_string_view` or `std::basic_string`
  specialization and `TD::char_type` is `Context::char_type`, the
  `std::basic_format_arg` stores
  `std::basic_string_view<Context::char_type>(t.data(), t.size())`;
- otherwise, if `std::decay_t<TD>` is `Context::char_type*` or `const
  Context::char_type*`, the `std::basic_format_arg` stores `static_cast<const
  Context::char_type*>(t)`;
- otherwise, if `std::is_void_v<std::remove_pointer_t<TD>>` is `true` or
  `std::is_null_pointer_v<TD>` is `true`, the `std::basic_format_arg` stores
  `static_cast<const void*>(t)`;
- otherwise, the `std::basic_format_arg` stores a
  `std::basic_format_arg<Context>::handle` to `t`, along with extra data needed
  for `handle::format()`.

### Notes

A formatting argument has reference semantics for user-defined types and does
not extend the lifetime of `args`. It is the programmer's responsibility to
ensure that `args` outlive the return value. Usually, the result is only used as
argument to formatting function.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_format_uchar` | 202311L | (C++20) (DR) | Formatting of code units
      as unsigned integers

### Example

```cpp
#include <array>
#include <format>
#include <iostream>
#include <string_view>

void raw_write_to_log(std::string_view users_fmt, std::format_args&& args)
{
    static int n{};
    std::clog << std::format("{:04} : ", n++) << std::vformat(users_fmt, args) << '\n';
}

template<typename... Args>
constexpr void log(Args&&... args)
{
    // Generate formatting string "{} "...
    std::array<char, sizeof...(Args) * 3 + 1> braces{};
    constexpr const char c[4] = "{} ";
    for (auto i{0uz}; i != braces.size() - 1; ++i)
        braces[i] = c[i % 3];
    braces.back() = '\0';

    raw_write_to_log(std::string_view{braces.data()}, std::make_format_args(args...));
}

int main()
{
    log("Number", "of", "arguments", "is", "arbitrary.");
    log("Any type that meets the BasicFormatter requirements", "can be printed.");
    log("For example:", 1, 2.0, '3', "*42*");

    raw_write_to_log("{:02} │ {} │ {} │ {}", std::make_format_args(1, 2.0, '3', "4"));
}
```

Output:

```text
0000 : Number of arguments is arbitrary.
0001 : Any type that meets the BasicFormatter requirements can be printed.
0002 : For example: 1 2.0 3 *42*
0003 : 01 │ 2.0 │ 3 │ 4
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2418R2 | C++20 | objects that are neither const-usable nor copyable (such as
      generator-like objects) are not formattable | allow formatting these
      objects
  P2905R2 | C++20 | `make_format_args` accepted rvalue arguments by forwarding
      references | only takes lvalue references
  P2909R4 | C++20 | char or wchar_t might be formatted as out-of-range unsigned
      integer values | code units are converted to the corresponding unsigned
      type before such formatting
  LWG 3631 | C++20 | cv-qualified arguments were incorrectly handled after
      P2418R2 | handling corrected

### See also

- **basic_format_argsformat_argswformat_args (C++20)(C++20)(C++20)** — class
  that provides access to all formatting arguments (class template)
- **vformat (C++20)** — non-template variant of `std::format` using type-erased
  argument representation (function)
- **vformat_to (C++20)** — non-template variant of `std::format_to` using
  type-erased argument representation (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/format/make_format_args*
