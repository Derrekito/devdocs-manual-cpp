# std::allocator<T>::destroy

```cpp
void destroy( pointer p );  // (1) (until C++11)
template< class U >
void destroy( U* p );  // (2) (since C++11) (deprecated in C++17) (removed in C++20)
```

Calls the destructor of the object pointed to by `p`.

1) Calls `p->~T()`.

2) Calls `p->~U()`.

### Parameters

- **p** — pointer to the object that is going to be destroyed

### Return value

(none)

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 400 | C++98 | `p` was cast to `T*`, which is the type of `p` | removed the
      redundant cast

### See also

- **destroy [static]** — destructs an object stored in the allocated storage
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/allocator/destroy*
