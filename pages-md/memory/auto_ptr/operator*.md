# std::auto_ptr<T>::operator*, std::auto_ptr<T>::operator->

```cpp
T& operator*() const throw();  // (1) (deprecated in C++11) (removed in C++17)
T* operator->() const throw();  // (2) (deprecated in C++11) (removed in C++17)
```

Dereferences a pointer to the managed object. The first version requires `get()
!= 0`.

### Parameters

(none)

### Return value

1) `*get()`.

2) `get()`.

### See also

- **get** — returns a pointer to the managed object (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/auto_ptr/operator**
