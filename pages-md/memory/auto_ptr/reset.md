# std::auto_ptr<T>::reset

```cpp
void reset( T* p = 0 ) throw();  // (deprecated in C++11) (removed in C++17)
```

Replaces the held pointer by `p`. If the currently held pointer is not null
pointer, `delete get()` is called.

### Parameters

- **p** — a pointer to an object to manage

### Return value

(none)

### See also

- **release** — releases ownership of the managed object (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/memory/auto_ptr/reset*
