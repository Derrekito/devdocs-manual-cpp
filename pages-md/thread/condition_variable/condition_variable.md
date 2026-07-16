# std::condition_variable::condition_variable

```cpp
condition_variable();  // (1) (since C++11)
condition_variable( const condition_variable& ) = delete;  // (2) (since C++11)
```

1) Constructs an object of type `std::condition_variable`.

2) Copy constructor is deleted.

### Parameters

(none)

### Exceptions

1) May throw `std::system_error` with `std::error_condition` equal to
   `std::errc::operation_not_permitted` if the thread has no privilege to create
   a condition variable, `std::errc::resource_unavailable_try_again` if a
   non-memory resource limitation prevents this initialization, or another
   implementation-defined value.

### See also

**C documentation for `cnd_init`**

---
*Source: https://en.cppreference.com/w/cpp/thread/condition_variable/condition_variable*
