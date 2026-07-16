# std::condition_variable::native_handle

```cpp
native_handle_type native_handle();  // (since C++11)
```

Accesses the native handle of `*this`.

The meaning and the type of the result of this function is
implementation-defined. On a POSIX system, this may be a value of type
`pthread_cond_t*`. On a Windows system, this may be a `PCONDITION_VARIABLE`.

### Parameters

(none)

### Return value

The native handle of this condition variable.

### See also

- **native_handle** — returns the underlying implementation-defined thread
  handle (public member function of `std::thread`)
- **native_handle** — returns the underlying implementation-defined thread
  handle (public member function of `std::jthread`)

---
*Source: https://en.cppreference.com/w/cpp/thread/condition_variable/native_handle*
