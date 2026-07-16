# std::shared_mutex::~shared_mutex

```cpp
~shared_mutex();
```

Destroys the mutex.

The behavior is undefined if the mutex is owned by any thread or if any thread
terminates while holding any ownership of the mutex.

### See also

**C documentation for `mtx_destroy`**

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_mutex/%7Eshared_mutex*
