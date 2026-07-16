# std::atomic_ref<T>::operator+=,-=,&=,|=,^=

```cpp
member only of atomic_ref<Integral> and atomic_ref<Floating> template specializations
T operator+=( T arg ) const noexcept;  // (1)
member only of atomic_ref<T*> template specialization
T* operator+=( std::ptrdiff_t arg ) const noexcept;  // (1)
member only of atomic_ref<Integral> and atomic_ref<Floating> template specializations
T operator-=( T arg ) const noexcept;  // (1)
member only of atomic_ref<T*> template specialization
T* operator-=( std::ptrdiff_t arg ) const noexcept;  // (1)
member only of atomic_ref<Integral> template specialization
T operator&=( T arg ) const noexcept;  // (3)
T operator|=( T arg ) const noexcept;  // (4)
T operator^=( T arg ) const noexcept;  // (5)
```

Atomically replaces the current value of the referenced object with the result
of computation involving the previous value and `arg`. These operations are
read-modify-write operations.

1) Performs atomic addition. Equivalent to `return fetch_add(arg) + arg;`.

2) Performs atomic subtraction. Equivalent to `return fetch_sub(arg) - arg;`.

3) Performs atomic bitwise and. Equivalent to `return fetch_and(arg) & arg;`.

4) Performs atomic bitwise or. Equivalent to `return fetch_or(arg) | arg;`.

5) Performs atomic bitwise exclusive or. Equivalent to `return fetch_xor(arg) ^
   arg;`.

For signed integral types, arithmetic is defined to use two’s complement
representation. There are no undefined results.

For floating-point types, the floating-point environment in effect may be
different from the calling thread's floating-point environment. The operation
need not be conform to the corresponding `std::numeric_limits` traits but is
encouraged to do so. If the result is not a representable value for its type,
the result is unspecified but the operation otherwise has no undefined behavior.

For `T*` types, the result may be an undefined address, but the operations
otherwise have no undefined behavior. The program is ill-formed if `T` is not an
object type.

### Parameters

- **arg** — the argument for the arithmetic operation

### Return value

The resulting value (that is, the result of applying the corresponding binary
operator to the value immediately preceding the effects of the corresponding
member function).

### Notes

Unlike most compound assignment operators, the compound assignment operators for
`atomic_ref` do not return a reference to their left-hand arguments. They return
a copy of the stored value instead.

### See also

- **operator++operator++(int)operator--operator--(int)** — atomically increments
  or decrements the referenced object by one (public member function)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_ref/operator_arith2*
