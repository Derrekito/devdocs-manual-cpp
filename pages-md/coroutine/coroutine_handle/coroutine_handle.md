# std::coroutine_handle<Promise>::coroutine_handle

```cpp
constexpr coroutine_handle() noexcept;  // (1) (since C++20)
constexpr coroutine_handle( std::nullptr_t ) noexcept;  // (2) (since C++20)
coroutine_handle( const coroutine_handle& other ) = default;  // (3) (since C++20)
coroutine_handle( coroutine_handle&& other ) = default;  // (4) (since C++20)
```

Creates a `coroutine_handle` that does not refer a coroutine, or copies a
`coroutine_handle`.

1,2) Initializes the underlying address `ptr` to `nullptr`. After construction,
   `address()` returns `nullptr`, and the `coroutine_handle` does not refer a
   coroutine. These constructors are not declared for the specialization
   `std::coroutine_handle<std::noop_coroutine_promise>`.

3,4) Copies the underlying address. The copy constructor and move constructor
   are equivalent to implicitly declared ones.

### Parameters

- **other** — another `coroutine_handle` to copy

### Notes

`std::coroutine_handle<std::noop_coroutine_promise>` is neither default
constructible nor constructible from `std::nullptr_t`. `std::noop_coroutine` can
be used to create a new `std::coroutine_handle<std::noop_coroutine_promise>`.

Static member functions `from_promise` and `from_address` can also create a
`coroutine_handle`.

### See also

- **from_promise [static] (C++20)** — creates a `coroutine_handle` from the
  promise object of a coroutine (public static member function)
- **from_address [static] (C++20)** — imports a coroutine from a pointer (public
  static member function)
- **noop_coroutine (C++20)** — creates a coroutine handle that has no observable
  effects when resumed or destroyed (function)

---
*Source: https://en.cppreference.com/w/cpp/coroutine/coroutine_handle/coroutine_handle*
