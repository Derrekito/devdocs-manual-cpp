# std::shared_future<T>::~shared_future

```cpp
~shared_future();  // (since C++11)
```

If `*this` is the last object referring to the shared state, destroys the shared
state. Otherwise does nothing.

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_future/%7Eshared_future*
