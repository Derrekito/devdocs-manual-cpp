# std::future<T>::operator=

```cpp
future& operator=( future&& other ) noexcept;  // (1) (since C++11)
future& operator=( const future& other ) = delete;  // (2) (since C++11)
```

Assigns the contents of another future object.

1) Releases any shared state and move-assigns the contents of `other` to
   `*this`. After the assignment, `other.valid() == false` and `this->valid()`
   will yield the same value as `other.valid()` before the assignment.

2) `std::future` is not CopyAssignable.

### Parameters

- **other** — a `std::future` that will transfer state to `*this`

### Return value

`*this`

---
*Source: https://en.cppreference.com/w/cpp/thread/future/operator%3D*
