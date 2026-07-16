# std::function<R(Args...)>::swap

```cpp
void swap( function& other ) noexcept;  // (since C++11)
```

Exchanges the stored callable objects of `*this` and `other`.

### Parameters

- **other** — function wrapper to exchange the stored callable object with

### Return value

(none)

### See also

- **swap (C++23)** — swaps the targets of two `std::move_only_function` objects
  (public member function of `std::move_only_function`)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/function/swap*
