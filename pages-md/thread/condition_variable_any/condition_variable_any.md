# std::condition_variable_any::condition_variable_any

```cpp
condition_variable_any();  // (1) (since C++11)
condition_variable_any( const condition_variable_any& ) = delete;  // (2) (since C++11)
```

1) Constructs an object of type `std::condition_variable_any`.

2) Copy constructor is deleted.

### Parameters

(none)

### Exceptions

1) May throw `std::system_error` with `std::error_condition` equal to
   `std::errc::operation_not_permitted` if the thread has no privilege to create
   a condition variable, `std::errc::resource_unavailable_try_again` if a
   non-memory resource limitation prevents this initialization, or another
   implementation-defined value.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2092 | C++11 | the error condition for `resource_unavailable_try_again`
      was wrong | corrected

### See also

**C documentation for `cnd_init`**

---
*Source: https://en.cppreference.com/w/cpp/thread/condition_variable_any/condition_variable_any*
