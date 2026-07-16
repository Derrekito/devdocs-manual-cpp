# std::stop_token::operator=

```cpp
std::stop_token& operator=( const std::stop_token& other ) noexcept;  // (1) (since C++20)
std::stop_token& operator=( std::stop_token&& other ) noexcept;  // (2) (since C++20)
```

Replaces the associated stop-state with that of `other`.

1) Copy-assigns the associated stop-state of `other` to that of `*this`.
   Equivalent to `stop_token(other).swap(*this)`.

2) Move-assigns the associated stop-state of `other` to that of `*this`. After
   the assignment, `*this` contains the previous associated stop-state of
   `other`, and `other` has no associated stop-state. Equivalent to
   `stop_token(std::move(other)).swap(*this)`.

### Parameters

- **other** — another `stop_token` object to share the stop-state with to or
  acquire the stop-state from

---
*Source: https://en.cppreference.com/w/cpp/thread/stop_token/operator%3D*
