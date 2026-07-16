# std::error_code::operator=

```cpp
template< class ErrorCodeEnum >
error_code& operator=( ErrorCodeEnum e ) noexcept;  // (1) (since C++11)
error_code& operator=( const error_code& other ) = default;  // (2) (since C++11) (implicitly declared)
error_code& operator=( error_code&& other ) = default;  // (3) (since C++11) (implicitly declared)
```

1) Replaces the error code and corresponding category with those representing
   error code enum `e`. Equivalent to `*this = make_error_code(e)`, where
   `make_error_code` is only found by argument-dependent lookup. This overload
   participates in overload resolution only if
   `std::is_error_code_enum<ErrorCodeEnum>::value` is `true`.

2,3) Implicitly defined copy-assignment operator and move-assignment operator
   assign the contents of `other` to `*this`.

### Parameters

- **e** — error code enum to construct
- **other** — another error code to assign with

### Return value

`*this`

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3629 | C++11 | only `std::make_error_code` overloads were used | ADL-found
      overloads are used

### See also

- **assign** — assigns another error code (public member function)

---
*Source: https://en.cppreference.com/w/cpp/error/error_code/operator%3D*
