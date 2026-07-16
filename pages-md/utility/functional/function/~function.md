# std::function<R(Args...)>::~function

```cpp
~function();  // (since C++11)
```

Destroys the `std::function` instance. If the `std::function` is not *empty*,
its *target* is destroyed also.

### See also

- **(destructor) (C++23)** — destroys a `std::move_only_function` object (public
  member function of `std::move_only_function`)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/function/%7Efunction*
