# std::atomic_init

```cpp
template< class T >
void atomic_init
    ( std::atomic<T>* obj,
      typename std::atomic<T>::value_type desired ) noexcept;  // (1) (since C++11) (deprecated in C++20)
template< class T >
void atomic_init
    ( volatile std::atomic<T>* obj,
      typename std::atomic<T>::value_type desired ) noexcept;  // (2) (since C++11) (deprecated in C++20)
```

Initializes the default-constructed atomic object `obj` with the value
`desired`. The function is not atomic: concurrent access from another thread,
even through an atomic operation, is a data race.

If `obj` was not default-constructed, or this function is called twice on the
same `obj`, the behavior is undefined.

### Parameters

- **obj** — pointer to an atomic object to initialize
- **desired** — the value to initialize atomic object with

### Return value

(none)

### Notes

This function is provided for compatibility with C. If the compatibility is not
required, `std::atomic` may be initialized through their non-default
constructors.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0558R1 | C++11 | exact type match was required because `T` was deduced from
      multiple arguments | `T` is only deduced from `obj`

### See also

- **ATOMIC_VAR_INIT (C++11)(deprecated in C++20)** — constant initialization of
  an atomic variable of static storage duration (function macro)
- **(constructor)** — constructs an atomic object (public member function of
  `std::atomic<T>`)

**C documentation for `atomic_init`**

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_init*
