# std::move_only_function::swap

```cpp
void swap( move_only_function& other ) noexcept;  // (since C++23)
```

Exchanges the stored callable objects of `*this` and `other`.

### Parameters

- **other** — function wrapper to exchange the stored callable object with

### Return value

(none)

### See also

- **swap** — swaps the contents (public member function of
  `std::function<R(Args...)>`)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/move_only_function/swap*
