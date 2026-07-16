# std::counting_semaphore<LeastMaxValue>::counting_semaphore

```cpp
constexpr explicit counting_semaphore( std::ptrdiff_t desired );  // (1) (since C++20)
counting_semaphore( const counting_semaphore& ) = delete;  // (2) (since C++20)
```

1) Constructs an object of type `std::counting_semaphore` with the internal
   counter initialized to `desired`.

2) Copy constructor is deleted.

### Preconditions

1) Both `desired >= 0` and `desired <= max()` are `true`.

### Parameters

- **desired** — the value to initialize `counting_semaphore`'s counter with

### Exceptions

Throws nothing.

---
*Source: https://en.cppreference.com/w/cpp/thread/counting_semaphore/counting_semaphore*
