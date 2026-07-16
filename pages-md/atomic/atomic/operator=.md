# std::atomic<T>::operator=

```cpp
T operator=( T desired ) noexcept;  // (1) (since C++11)
T operator=( T desired ) volatile noexcept;  // (2) (since C++11)
atomic& operator=( const atomic& ) = delete;  // (3) (since C++11)
atomic& operator=( const atomic& ) volatile = delete;  // (4) (since C++11)
```

1,2) Atomically assigns `desired` to the atomic variable. Equivalent to
   `store(desired)`. It is deprecated if `std::atomic<T>::is_always_lock_free`
   is `false` and overload (2) participates in overload resolution. (since
   C++20)

3,4) Atomic variables are not CopyAssignable.

### Parameters

- **desired** — value to assign

### Return value

1,2) `desired`

### Notes

Unlike most assignment operators, the assignment operators for atomic types do
not return a reference to their left-hand arguments. They return a copy of the
stored value instead.

### See also

- **(constructor)** — constructs an atomic object (public member function)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic/operator%3D*
