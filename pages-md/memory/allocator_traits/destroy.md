# std::allocator_traits<Alloc>::destroy

```cpp
template< class T >
static void destroy( Alloc& a, T* p );  // (since C++11) (until C++20)
template< class T >
static constexpr void destroy( Alloc& a, T* p );  // (since C++20)
```

Calls the destructor of the object pointed to by `p`. If possible, does so by
calling `a.destroy(p)`. If not possible (e.g. `Alloc` does not have the member
function `destroy()`), then calls the destructor of `*p` directly, as
`p->~T()`(until C++20)`std::destroy_at(p)`(since C++20).

### Parameters

- **a** — allocator to use for destruction
- **p** — pointer to the object being destroyed

### Return value

(none)

### Notes

Because this function provides the automatic fall back to direct call to the
destructor, the member function `destroy()` is an optional Allocator requirement
since C++11.

### Example

### See also

- **destroy (until C++20)** — destructs an object in allocated storage (public
  member function of `std::allocator<T>`)

---
*Source: https://en.cppreference.com/w/cpp/memory/allocator_traits/destroy*
