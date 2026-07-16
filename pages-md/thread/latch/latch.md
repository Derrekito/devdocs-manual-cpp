# std::latch::latch

```cpp
constexpr explicit latch( std::ptrdiff_t expected );  // (1) (since C++20)
latch( const latch& ) = delete;  // (2) (since C++20)
```

1) Constructs a `latch` and initializes its internal counter. The behavior is
   undefined if `expected` is negative or greater than `max()`.

2) Copy constructor is deleted. `latch` is neither copyable nor movable.

### Parameters

- **expected** — the initial value of the internal counter

### Exceptions

Throws nothing.

---
*Source: https://en.cppreference.com/w/cpp/thread/latch/latch*
