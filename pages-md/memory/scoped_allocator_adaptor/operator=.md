# std::scoped_allocator_adaptor<OuterAlloc,InnerAlloc...>::operator=

```cpp
scoped_allocator_adaptor& operator=( const scoped_allocator_adaptor& other ) = default;  // (1)
scoped_allocator_adaptor& operator=( scoped_allocator_adaptor&& other ) = default;  // (2)
```

1) Explicitly defaulted copy assignment operator that copy assigns the base
   class (`OuterAlloc`, the outer allocator) and all inner allocators.

2) Explicitly defaulted move assignment operator that move assigns the base
   class (`OuterAlloc`, the outer allocator) and all inner allocators.

### Parameters

- **other** — another `std::scoped_allocator_adaptor`

---
*Source: https://en.cppreference.com/w/cpp/memory/scoped_allocator_adaptor/operator%3D*
