# std::promise<R>::operator=

```cpp
promise& operator=( promise&& other ) noexcept;  // (1) (since C++11)
promise& operator=( const promise& rhs ) = delete;  // (2) (since C++11)
```

Assigns the contents.

1) Move assignment operator. First, abandons the shared state (as in
   `~promise()`), then assigns the shared state of `other` as if by executing
   `std::promise(std::move(other)).swap(*this)`.

2) `promise` is not copy-assignable.

### Parameters

- **other** — another `promise` to acquire the state from

### Return value

`*this`

---
*Source: https://en.cppreference.com/w/cpp/thread/promise/operator%3D*
