# std::atomic_ref<T>::fetch_sub

```cpp
member only of atomic_ref<Integral> and atomic_ref<Floating> template specializations
T fetch_sub( T arg, std::memory_order order = std::memory_order_seq_cst ) const noexcept;  // (1)
member only of atomic_ref<T*> template specialization
T* fetch_sub( std::ptrdiff_t arg,
              std::memory_order order = std::memory_order_seq_cst ) const noexcept;  // (2)
```

Atomically replaces the current value of the referenced object with the result
of arithmetic subtraction of the value and `arg`. This operation is a
read-modify-write operation. Memory is affected according to the value of
`order`.

For signed integral types, arithmetic is defined to use two’s complement
representation. There are no undefined results.

For floating-point types, the floating-point environment in effect may be
different from the calling thread's floating-point environment. The operation
need not be conform to the corresponding `std::numeric_limits` traits but is
encouraged to do so. If the result is not a representable value for its type,
the result is unspecified but the operation otherwise has no undefined behavior.

For `T*` types, the result may be an undefined address, but the operation
otherwise has no undefined behavior. The program is ill-formed if `T` is not an
object type.

### Parameters

- **arg** — the other argument of arithmetic subtraction
- **order** — memory order constraints to enforce

### Return value

The value of the referenced object, immediately preceding the effects of this
function.

### Example

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_ref/fetch_sub*
