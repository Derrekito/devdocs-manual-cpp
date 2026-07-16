# std::scoped_allocator_adaptor<OuterAlloc,InnerAlloc...>::outer_allocator

```cpp
outer_allocator_type& outer_allocator() noexcept;  // (1) (since C++11)
const outer_allocator_type& outer_allocator() const noexcept;  // (2) (since C++11)
```

Obtains a reference to the outer allocator used to declare this class.

1) Returns `static_cast<OuterAlloc&>(*this)`.

2) Returns `static_cast<const OuterAlloc&>(*this)`.

### Parameters

(none)

### Return value

A reference to `OuterAlloc`.

### See also

- **inner_allocator** — obtains an `inner_allocator` reference (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/memory/scoped_allocator_adaptor/outer_allocator*
