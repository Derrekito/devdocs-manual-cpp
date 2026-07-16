# std::auto_ptr<T>::operator auto_ptr<Y>

```cpp
template< class Y >
operator auto_ptr_ref<Y>() throw();  // (1) (deprecated in C++11) (removed in C++17)
template< class Y >
operator auto_ptr<Y>() throw();  // (2) (deprecated in C++11) (removed in C++17)
```

Converts `*this` to an `auto_ptr` for a different type `Y`.

1) Returns an implementation-defined type that holds a reference to `*this`.
   `std::auto_ptr` is convertible and assignable from this template. The
   implementation is allowed to provide the template with a different name or
   implement equivalent functionality in other ways.

2) Constructs a new `auto_ptr` with a pointer obtained by calling `release()`.

### Parameters

(none)

### Return value

1) An implementation-defined type that holds a reference to `*this`.

2) An `auto_ptr` with a pointer obtained by calling `release()`.

### Notes

The constructor and the copy assignment operator from `auto_ptr_ref` is provided
to allow copy-constructing and assigning `std::auto_ptr` from nameless
temporaries. Since its copy constructor and copy assignment operator take the
argument as non-const reference, they cannot bind rvalue arguments directly.
However, a user-defined conversion (1) or (2) can be executed (which releases
the original `auto_ptr`), followed by a call to the constructor or
copy-assignment operator that take `auto_ptr_ref` by value. This is an early
implementation of move semantics.

---
*Source: https://en.cppreference.com/w/cpp/memory/auto_ptr/operator_auto_ptr*
