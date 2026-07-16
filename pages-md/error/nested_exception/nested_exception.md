# std::nested_exception::nested_exception

```cpp
nested_exception() noexcept;  // (1) (since C++11)
nested_exception( const nested_exception& other ) noexcept = default;  // (2) (since C++11)
```

Constructs new `nested_exception` object.

1) Default constructor. Stores an exception object obtained by calling
   `std::current_exception()` within the new `nested_exception` object.

2) Copy constructor. Initializes the object with the exception stored in
   `other`.

### Parameters

- **other** — nested exception to initialize the contents with

---
*Source: https://en.cppreference.com/w/cpp/error/nested_exception/nested_exception*
