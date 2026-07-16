# std::allocator<T>::address

```cpp
pointer address( reference x ) const;  // (until C++11)
pointer address( reference x ) const noexcept;  // (since C++11) (deprecated in C++17) (removed in C++20)
const_pointer address( const_reference x ) const;  // (until C++11)
const_pointer address( const_reference x ) const noexcept;  // (since C++11) (deprecated in C++17) (removed in C++20)
```

Returns the actual address of `x` even in presence of overloaded operator&.

### Parameters

- **x** — the object to acquire address of

### Return value

The actual address of `x`.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 634 (N2436) | C++98 | the return value is `&x` (which is affected by
      overloaded operator&) | returns the actual address of `x`

---
*Source: https://en.cppreference.com/w/cpp/memory/allocator/address*
