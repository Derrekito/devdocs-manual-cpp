# std::atomic_ref<T>::operator=

```cpp
T operator=( T desired ) const noexcept;  // (1)
atomic_ref& operator=( const atomic_ref& ) = delete;  // (2)
```

1) Atomically assigns a value `desired` to the referenced object. Equivalent to
   `store(desired)`.

2) `atomic_ref` is not CopyAssignable.

### Parameters

- **desired** — value to assign

### Return value

`desired`

### Notes

Unlike most assignment operators, the assignment operator for `atomic_ref` does
not return a reference to its left-hand argument. It returns a copy of the
stored value instead.

### See also

- **(constructor)** — constructs an `atomic_ref` object (public member function)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_ref/operator%3D*
