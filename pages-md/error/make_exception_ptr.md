# std::make_exception_ptr

```cpp
template< class E >
std::exception_ptr make_exception_ptr( E e ) noexcept;  // (since C++11)
```

Creates an `std::exception_ptr` that holds a reference to a copy of `e`. This is
done as if executing the following code:

```cpp
try {
    throw e;
} catch(...) {
    return std::current_exception();
}
```

### Parameters

(none)

### Return value

An instance of `std::exception_ptr` holding a reference to the copy of `e`, or
to an instance of `std::bad_alloc` or to an instance of `std::bad_exception`
(see `std::current_exception`).

### Notes

The parameter is passed by value and is subject to slicing.

### See also

- **current_exception (C++11)** — captures the current exception in a
  `std::exception_ptr` (function)

---
*Source: https://en.cppreference.com/w/cpp/error/make_exception_ptr*
