# std::error_condition::operator=

```cpp
template< class ErrorConditionEnum >
error_condition& operator=( ErrorConditionEnum e ) noexcept;  // (1) (since C++11)
error_condition& operator=( const error_condition& other ) = default;  // (2) (since C++11) (implicitly declared)
error_condition& operator=( error_condition&& other ) = default;  // (3) (since C++11) (implicitly declared)
```

Assigns contents to an error condition.

1) Assigns error condition for enum `e`. Effectively calls
   `make_error_condition` that is only found by argument-dependent lookup for
   `e` and then replaces `*this` with the result. This overload participates in
   overload resolution only if
   `std::is_error_condition_enum<ErrorConditionEnum>::value` is `true`.

2,3) Implicitly defined copy-assignment operator and move-assignment operator
   assign the contents of `other` to `*this`.

### Parameters

- **e** — error condition enum
- **other** — another error condition to assign with

### Return value

`*this`.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3629 | C++11 | only `std::make_error_condition` overloads were used |
      ADL-found overloads are used

---
*Source: https://en.cppreference.com/w/cpp/error/error_condition/operator%3D*
