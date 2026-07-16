# std::future_error::future_error

```cpp
future_error( const future_error& other ) noexcept;  // (1) (since C++11)
explicit future_error( std::future_errc ec );  // (2) (since C++17)
```

1) Copy constructor. Initializes the contents of the new `future_error` object
   with those of `other`. If `*this` and `other` both have dynamic type
   `std::future_error` then `std::strcmp(what(), other.what()) == 0`.

2) Constructs a new `future_error` object containing the error code
   `std::make_error_code(ec)`.

### Parameters

- **other** — another `future_error` object to copy
- **ec** — error code

### Notes

There is no standard-compliant way for the user to construct a `future_error`
other than copying from another `future_error` prior to C++17. C++11 and C++14
depict an exposition-only public constructor taking a `std::error_code`, and
some implementations provide such a constructor.

---
*Source: https://en.cppreference.com/w/cpp/thread/future_error/future_error*
