# std::atomic_ref<T>::operator++,++(int),--,--(int)

```cpp
member only of atomic_ref<Integral> and atomic_ref<T*> template specializations
value_type operator++() const noexcept;  // (1) (since C++20)
value_type operator++( int ) const noexcept;  // (2) (since C++20)
value_type operator--() const noexcept;  // (3) (since C++20)
value_type operator--( int ) const noexcept;  // (4) (since C++20)
```

Atomically increments or decrements the current value of the referenced object.
These operations are read-modify-write operations.

1) Performs atomic pre-increment. Equivalent to `return fetch_add(1) + 1;`.

2) Performs atomic post-increment. Equivalent to `return fetch_add(1);`.

3) Performs atomic pre-decrement. Equivalent to `return fetch_sub(1) - 1;`

4) Performs atomic post-decrement. Equivalent to `return fetch_sub(1);`.

For signed `Integral` types, arithmetic is defined to use two’s complement
representation. There are no undefined results.

For `T*` types, the result may be an undefined address, but the operations
otherwise have no undefined behavior. The program is ill-formed if `T` is not an
object type.

### Parameters

(none)

### Return value

1,3) The value of the referenced object after the modification.

2,4) The value of the referenced object before the modification.

### Notes

Unlike most pre-increment and pre-decrement operators, the pre-increment and
pre-decrement operators for `atomic_ref` do not return a reference to the
modified object. They return a copy of the stored value instead.

### See also

- **fetch_add** — atomically adds the argument to the value stored in the
  referenced object and obtains the value held previously (public member
  function)
- **fetch_sub** — atomically subtracts the argument from the value stored in the
  referenced object and obtains the value held previously (public member
  function)
- **operator+=operator-=operator&=operator|=operator^=** — atomically adds,
  subtracts, or performs bitwise AND, OR, XOR with the referenced value (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_ref/operator_arith*
