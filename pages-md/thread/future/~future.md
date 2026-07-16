# std::future<T>::~future

```cpp
~future();  // (since C++11)
```

Releases any shared state. This means

- if the current object holds the last reference to its shared state, the shared
  state is destroyed;
- the current object gives up its reference to its shared state;

- these actions will not block for the shared state to become ready, except that
  they may block if all of the following are true:
1. the shared state was created by a call to `std::async`,
1. the shared state is not yet ready, and
1. the current object was the last reference to the shared state.
*(since C++14)*

In practice, these actions will block only if the task’s launch policy is
`std::launch::async` (see "Effective Modern C++" Item 36), either because that
was chosen by the runtime system or because it was specified in the call to
`std::async`.

---
*Source: https://en.cppreference.com/w/cpp/thread/future/%7Efuture*
