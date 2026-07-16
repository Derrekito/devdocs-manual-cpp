# std::pointer_traits<Ptr>::pointer_to

```cpp
static pointer
pointer_to( element_type& r );  // (1) (since C++11) (member of pointer_traits<Ptr> specialization)
static pointer
pointer_to( element_type& r ) noexcept;  // (since C++11) (until C++20) (member of pointer_traits<T*> specialization)
static constexpr pointer
pointer_to( element_type& r ) noexcept;  // (since C++20) (member of pointer_traits<T*> specialization)
```

Constructs a dereferenceable pointer or pointer-like object ("fancy pointer") to
its argument.

1) The version of this function in the non-specialized `std::pointer_traits`
   template simply calls `Ptr::pointer_to(r)`, and if Ptr does not provide a
   static member function `pointer_to`, instantiation of this function is a
   compile-time error.

2) The version of this function in the specialization of `std::pointer_traits`
   for pointer types returns `std::addressof(r)`.

### Parameters

- **r** — reference to an object of type `element_type&`, except if element_type
  is `void`, in which case the type of `r` is unspecified

### Return value

A dereferenceable pointer to `r`, of the type `pointer_traits<>::pointer`.

### Exceptions

1) Unspecified (typically same as `Ptr::pointer_to`).

### Notes

The Boost.Intrusive library version of this function returns
`pointer(std::addressof(r))` if `Ptr::pointer_to` does not exist.

### See also

- **addressof (C++11)** — obtains actual address of an object, even if the `&`
  operator is overloaded (function template)
- **address (until C++20)** — obtains the address of an object, even if
  operator& is overloaded (public member function of `std::allocator<T>`)
- **to_address [static] (C++20)(optional)** — obtains a raw pointer from a fancy
  pointer (inverse of `pointer_to`) (public static member function)
- **to_address (C++20)** — obtains a raw pointer from a pointer-like type
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/pointer_traits/pointer_to*
