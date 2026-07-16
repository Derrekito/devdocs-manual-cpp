# std::scoped_allocator_adaptor<OuterAlloc,InnerAlloc...>::inner_allocator

```cpp
inner_allocator_type& inner_allocator() noexcept;  // (1) (since C++11)
const inner_allocator_type& inner_allocator() const noexcept;  // (2) (since C++11)
```

Obtains a reference to the inner allocator used to declare this
`scoped_allocator_adaptor`.

If `sizeof...(InnerAllocs) == 0`, that is, no inner allocators were declared,
returns `*this`. Otherwise returns a reference to
`std::scoped_allocator_adaptor<InnerAllocs...>`, that is, a scoped allocator
composed of all inner allocators of `*this`, with the first inner allocator
becoming the outer allocator.

### Parameters

(none)

### Return value

A reference to the inner allocator, which is itself a
`std::scoped_allocator_adaptor`.

### See also

- **outer_allocator** — obtains an `outer_allocator` reference (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/memory/scoped_allocator_adaptor/inner_allocator*
