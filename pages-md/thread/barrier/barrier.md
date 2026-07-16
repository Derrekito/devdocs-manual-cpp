# std::barrier<CompletionFunction>::barrier

```cpp
constexpr explicit barrier( std::ptrdiff_t expected,
                            CompletionFunction f = CompletionFunction());  // (1) (since C++20)
barrier( const barrier& ) = delete;  // (2) (since C++20)
```

1) Sets the both initial expected count for each phase and the current expected
   count for the first phase to `expected`, initializes the completion function
   object with `std::move(f)`, and then starts the first phase. The behavior is
   undefined if `expected` is negative or greater than `max()`.

2) Copy constructor is deleted. `barrier` is neither copyable nor movable.

### Parameters

- **expected** — initial value of the expected count
- **f** — completion function object to be called on phase completion step

### Exceptions

1) Any exception thrown by `CompletionFunction`'s move constructor.

### Notes

`expected` is permitted to be zero. However, calling any non-static member
function other than the destructor on such `barrier` results in undefined
behavior. In other words, such `barrier` can only be destroyed.

---
*Source: https://en.cppreference.com/w/cpp/thread/barrier/barrier*
