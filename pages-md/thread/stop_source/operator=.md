# std::stop_source::operator=

```cpp
std::stop_source& operator=( const std::stop_source& other ) noexcept;  // (1) (since C++20)
std::stop_source& operator=( std::stop_source&& other ) noexcept;  // (2) (since C++20)
```

Replaces the stop-state with that of `other`.

1) Copy-assigns the stop-state of `other` to that of `*this`. Equivalent to
   `stop_source(other).swap(*this)`.

2) Move-assigns the stop-state of `other` to that of `*this`. After the
   assignment, `*this` contains the previous stop-state of `other`, and `other`
   has no stop-state. Equivalent to `stop_source(std::move(other)).swap(*this)`.

### Parameters

- **other** — another `stop_source` object to share the stop-state with to or
  acquire the stop-state from

---
*Source: https://en.cppreference.com/w/cpp/thread/stop_source/operator%3D*
