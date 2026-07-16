# std::timed_mutex::~timed_mutex

```cpp
~timed_mutex();
```

Destroys the mutex.

The behavior is undefined if the mutex is owned by any thread or if any thread
terminates while holding any ownership of the mutex.

### See also

**C documentation for `mtx_destroy`**

---
*Source: https://en.cppreference.com/w/cpp/thread/timed_mutex/%7Etimed_mutex*
