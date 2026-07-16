# std::future<T>::future

```cpp
future() noexcept;  // (1) (since C++11)
future( future&& other ) noexcept;  // (2) (since C++11)
future( const future& other ) = delete;  // (3) (since C++11)
```

Constructs a `std::future` object.

1) Default constructor. Constructs a `std::future` with no shared state. After
   construction, `valid()` `== false`.

2) Move constructor. Constructs a `std::future` with the shared state of `other`
   using move semantics. After construction, `other.valid() == false`.

3) `std::future` is not CopyConstructible.

### Parameters

- **other** — another `std::future` to acquire shared state from

---
*Source: https://en.cppreference.com/w/cpp/thread/future/future*
