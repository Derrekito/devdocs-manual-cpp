# std::allocator_traits<Alloc>::construct

```cpp
template< class T, class... Args >
static void construct( Alloc& a, T* p, Args&&... args );  // (since C++11) (until C++20)
template< class T, class... Args >
static constexpr void construct( Alloc& a, T* p, Args&&... args );  // (since C++20)
```

If possible, constructs an object of type `T` in allocated uninitialized storage
pointed to by `p`, by calling `a.construct(p, std::forward<Args>(args)...)`.

If the above is not possible (e.g. `Alloc` does not have the member function
`construct()`), then calls

`::new (static_cast<void*>(p)) T(std::forward<Args>(args)...)`
*(until C++20)*

`std::construct_at(p, std::forward<Args>(args)...)`
*(since C++20)*

### Parameters

- **a** — allocator to use for construction
- **p** — pointer to the uninitialized storage on which a `T` object will be
  constructed
- **args...** — the constructor arguments to pass to `a.construct()` or to
  placement-new(until C++20)`std::construct_at()`(since C++20)

### Return value

(none)

### Notes

This function is used by the standard library containers when inserting,
copying, or moving elements.

Because this function provides the automatic fall back to placement new, the
member function `construct()` is an optional Allocator requirement since C++11.

### See also

- **operator newoperator new[]** — allocation functions (function)
- **construct (until C++20)** — constructs an object in allocated storage
  (public member function of `std::allocator<T>`)
- **construct_at (C++20)** — creates an object at a given address (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/memory/allocator_traits/construct*
