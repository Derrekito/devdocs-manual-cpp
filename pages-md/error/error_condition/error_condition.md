# std::error_condition::error_condition

```cpp
error_condition() noexcept;  // (1) (since C++11)
error_condition( int val, const error_category& cat ) noexcept;  // (2) (since C++11)
template< class ErrorConditionEnum >
error_condition( ErrorConditionEnum e ) noexcept;  // (3) (since C++11)
error_condition( const error_condition& other ) = default;  // (4) (since C++11) (implicitly declared)
error_condition( error_condition&& other ) = default;  // (5) (since C++11) (implicitly declared)
```

Constructs new error condition.

1) Default constructor. Initializes the error condition with generic category
   and error value `​0​`.

2) Initializes the error condition with error value `val` and error category
   `cat`.

3) Initializes the error condition with enum `e`. Effectively calls
   `make_error_condition` that is only found by argument-dependent lookup for
   `e`. This overload participates in overload resolution only if
   `std::is_error_condition_enum<ErrorConditionEnum>::value` is `true`.

4,5) Implicitly defined copy constructor and move constructor. Initializes the
   error condition with the contents of the `other`.

### Parameters

- **other** — another error condition to initialize with
- **val** — error value
- **cat** — error category
- **e** — error condition enum

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3629 | C++11 | only `std::make_error_condition` overloads were used |
      ADL-found overloads are used

### See also

- **make_error_condition(std::errc) (C++11)** — creates an error condition for
  an `errc` value `e` (function)
- **make_error_condition(std::io_errc) (C++11)** — constructs an iostream error
  condition (function)
- **make_error_condition(std::future_errc) (C++11)** — constructs a future
  `error_condition` (function)

---
*Source: https://en.cppreference.com/w/cpp/error/error_condition/error_condition*
