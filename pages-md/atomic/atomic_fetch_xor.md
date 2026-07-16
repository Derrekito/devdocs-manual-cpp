# std::atomic_fetch_xor, std::atomic_fetch_xor_explicit

```cpp
template< class T >
T atomic_fetch_xor( std::atomic<T>* obj,
                    typename std::atomic<T>::value_type arg ) noexcept;  // (1) (since C++11)
template< class T >
T atomic_fetch_xor( volatile std::atomic<T>* obj,
                    typename std::atomic<T>::value_type arg ) noexcept;  // (2) (since C++11)
template< class T >
T atomic_fetch_xor_explicit( std::atomic<T>* obj,
                             typename std::atomic<T>::value_type arg,
                             std::memory_order order) noexcept;  // (3) (since C++11)
template< class T >
T atomic_fetch_xor_explicit( volatile std::atomic<T>* obj,
                             typename std::atomic<T>::value_type arg,
                             std::memory_order order) noexcept;  // (4) (since C++11)
```

Atomically replaces the value pointed by `obj` with the result of bitwise XOR
between the old value of `obj` and `arg`. Returns the value `obj` held
previously.

The operation is performed as if the following is executed:

1,2) `obj->fetch_xor(arg)`

3,4) `obj->fetch_xor(arg, order)`

If `std::atomic<T>` has no `fetch_xor` member (this member is only provided for
integral types except bool), the program is ill-formed.

### Parameters

- **obj** — pointer to the atomic object to modify
- **arg** — the value to bitwise XOR to the value stored in the atomic object
- **order** — the memory synchronization ordering

### Return value

The value immediately preceding the effects of this function in the modification
order of `*obj`.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0558R1 | C++11 | exact type match was required because `T` was deduced from
      multiple arguments | `T` is only deduced from `obj`

### See also

- **fetch_xor** — atomically performs bitwise XOR between the argument and the
  value of the atomic object and obtains the value held previously (public
  member function of `std::atomic<T>`)
- **atomic_fetch_oratomic_fetch_or_explicit (C++11)(C++11)** — replaces the
  atomic object with the result of bitwise OR with a non-atomic argument and
  obtains the previous value of the atomic (function template)
- **atomic_fetch_andatomic_fetch_and_explicit (C++11)(C++11)** — replaces the
  atomic object with the result of bitwise AND with a non-atomic argument and
  obtains the previous value of the atomic (function template)

**C documentation for `atomic_fetch_xor, atomic_fetch_xor_explicit`**

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_fetch_xor*
