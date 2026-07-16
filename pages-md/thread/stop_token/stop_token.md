# std::stop_token::stop_token

```cpp
stop_token() noexcept;  // (1) (since C++20)
stop_token( const stop_token& other ) noexcept;  // (2) (since C++20)
stop_token( stop_token&& other ) noexcept;  // (3) (since C++20)
```

Constructs a new `stop_token` object.

1) Constructs an empty `stop_token` with no associated stop-state.

2) Copy constructor. Constructs a `stop_token` whose associated stop-state is
   the same as that of `other`.

3) Move constructor. Constructs a `stop_token` whose associated stop-state is
   the same as that of `other`; `other` is left empty.

### Parameters

- **other** — another `stop_token` object to construct this `stop_token` object
  with

### Postconditions

1) `stop_possible()` and `stop_requested()` are both `false`.

2) `*this` and `other` share the same associated stop-state and compare equal.

3) `*this` has `other`'s previously associated stop-state, and
   `other.stop_possible()` is `false`.

---
*Source: https://en.cppreference.com/w/cpp/thread/stop_token/stop_token*
