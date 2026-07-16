# std::shared_future<T>::operator=

```cpp
shared_future& operator=( const shared_future& other );  // (since C++11) (until C++17)
shared_future& operator=( const shared_future& other ) noexcept;  // (since C++17)
shared_future& operator=( shared_future&& other ) noexcept;  // (2) (since C++11)
```

Assigns the contents of another `shared_future`.

1) Releases any shared state and assigns the contents of `other` to `*this`.
   After the assignment, `this->valid() == other.valid()`.

2) Releases any shared state and move-assigns the contents of `other` to
   `*this`. After the assignment, `other.valid() == false` and `this->valid()`
   will yield the same value as `other.valid()` before the assignment.

### Parameters

- **other** — a `std::shared_future` that will transfer state to `*this`

### Return value

`*this`

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_future/operator%3D*
