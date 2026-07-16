# std::promise<R>::~promise

```cpp
~promise();  // (since C++11)
```

Abandons the shared state:

- if the shared state is ready, releases it.
- if the shared state is not ready, stores an exception object of type
  `std::future_error` with an error condition
  `std::future_errc::broken_promise`, makes the shared state ready and releases
  it.

---
*Source: https://en.cppreference.com/w/cpp/thread/promise/%7Epromise*
