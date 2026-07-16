# std::future_errc

```cpp
enum class future_errc {
    broken_promise             = /* implementation-defined */,
    future_already_retrieved   = /* implementation-defined */,
    promise_already_satisfied  = /* implementation-defined */,
    no_state                   = /* implementation-defined */
};  // (since C++11)
```

The scoped enumeration `std::future_errc` defines the error codes reported by
`std::future` and related classes in `std::future_error` exception objects. Only
four error codes are required, although the implementation may define additional
error codes. Because the appropriate specialization of `std::is_error_code_enum`
is provided, values of type `std::future_errc` are implicitly convertible to
`std::error_code`.

All error codes are distinct and non-zero.

### Member constants

- **`broken_promise`** — the asynchronous task abandoned its shared state
- **`future_already_retrieved`** — the contents of shared state were already
  accessed through `std::future`
- **`promise_already_satisfied`** — attempt to store a value in the shared state
  twice
- **`no_state`** — attempt to access `std::promise` or `std::future` without an
  associated shared state

### Non-member functions

- **make_error_code(std::future_errc) (C++11)** — constructs a future error code
  (function)
- **make_error_condition(std::future_errc) (C++11)** — constructs a future
  `error_condition` (function)

### Helper classes

- **is_error_code_enum<std::future_errc> (C++11)** — extends the type trait
  `std::is_error_code_enum` to identify future error codes (class template)

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2056 | C++11 | `broken_promise` was specified to be zero which is
      conventionally used to mean "no error" | specified to be non-zero

### See also

- **error_code (C++11)** — holds a platform-dependent error code (class)
- **error_condition (C++11)** — holds a portable error code (class)

---
*Source: https://en.cppreference.com/w/cpp/thread/future_errc*
