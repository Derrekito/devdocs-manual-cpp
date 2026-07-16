# std::shared_future<T>::shared_future

```cpp
shared_future() noexcept;  // (1) (since C++11)
shared_future( const shared_future& other );  // (since C++11) (until C++17)
shared_future( const shared_future& other ) noexcept;  // (since C++17)
shared_future( std::future<T>&& other ) noexcept;  // (3) (since C++11)
shared_future( shared_future&& other ) noexcept;  // (4) (since C++11)
```

Constructs a new `shared_future`.

1) Default constructor. Constructs an empty shared future, that doesn't refer to
   a shared state, that is `valid() == false`.

2) Constructs a shared future that refers to the same shared state, if any, as
   `other`.

3,4) Transfers the shared state held by `other` to `*this`. After the
   construction, `other.valid() == false`, and `this->valid()` returns the same
   value as `other.valid()` would have returned before the construction.

### Parameters

- **other** — another future object to initialize with

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_future/shared_future*
