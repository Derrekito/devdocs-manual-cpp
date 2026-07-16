# std::jthread::operator=

```cpp
std::jthread& operator=( std::jthread&& other ) noexcept;  // (since C++20)
```

If `*this` still has an associated running thread (i.e. `joinable() == true`),
calls `request_stop()` followed by `join()`. Assigns the state of `other` to
`*this` and sets `other` to a default constructed state.

After this call, `this->get_id()` is equal to the value of `other.get_id()`
prior to the call and the associated stop-state is also moved, and `other` no
longer represents a thread of execution nor has any stop-state.

### Parameters

- **other** — another `jthread` object to assign to this `jthread` object

### Return value

`*this`

---
*Source: https://en.cppreference.com/w/cpp/thread/jthread/operator%3D*
