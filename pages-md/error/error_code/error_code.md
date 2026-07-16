# std::error_code::error_code

```cpp
error_code() noexcept;  // (1) (since C++11)
error_code( int ec, const error_category& ecat ) noexcept;  // (2) (since C++11)
template< class ErrorCodeEnum >
error_code( ErrorCodeEnum e ) noexcept;  // (3) (since C++11)
error_code( const error_code& other ) = default;  // (4) (since C++11) (implicitly declared)
error_code( error_code&& other ) = default;  // (5) (since C++11) (implicitly declared)
```

Constructs new error code.

1) Constructs error code with default value. Equivalent to `error_code(0,
   std::system_category())`.

2) Constructs error code with `ec` as the platform-dependent error code and
   `ecat` as the corresponding error category.

3) Constructs error code from an error code enum `e`. Equivalent to
   `make_error_code(e)`, where `make_error_code` is only found by
   argument-dependent lookup. This overload participates in overload resolution
   only if `std::is_error_code_enum<ErrorCodeEnum>::value` is `true`.

4,5) Implicitly defined copy constructor and move constructor. Initializes the
   error code with the contents of the `other`.

### Parameters

- **other** — another error code to initialize with
- **ec** — platform dependent error code to construct with
- **ecat** — error category corresponding to `ec`
- **e** — error code enum to construct with

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3629 | C++11 | only `std::make_error_code` overloads were used | ADL-found
      overloads are used

### See also

- **make_error_code(std::errc) (C++11)** — creates error code value for `errc`
  enum `e` (function)
- **make_error_code(std::io_errc) (C++11)** — constructs an iostream error code
  (function)
- **make_error_code(std::future_errc) (C++11)** — constructs a future error code
  (function)

---
*Source: https://en.cppreference.com/w/cpp/error/error_code/error_code*
