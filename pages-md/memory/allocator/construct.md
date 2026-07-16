# std::allocator<T>::construct

```cpp
void construct( pointer p, const_reference val );  // (1) (until C++11)
template< class U, class... Args >
void construct( U* p, Args&&... args );  // (2) (since C++11) (deprecated in C++17) (removed in C++20)
```

Constructs an object of type `T` in allocated uninitialized storage pointed to
by `p`, using global placement-new.

1) Calls `::new((void*)p) T(val)`.

2) Calls `::new((void*)p) U(std::forward<Args>(args)...)`.

### Parameters

- **p** — pointer to allocated uninitialized storage
- **val** — the value to use as the copy constructor argument
- **args...** — the constructor arguments to use

### Return value

(none)

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 402 | C++98 | if T::operator new exists, the program might be ill-formed |
      uses global replacement-new instead

### See also

- **construct [static]** — constructs an object in the allocated storage
  (function template)
- **construct_at (C++20)** — creates an object at a given address (function
  template)
- **operator newoperator new[]** — allocation functions (function)

---
*Source: https://en.cppreference.com/w/cpp/memory/allocator/construct*
