# deduction guides for `std::scoped_allocator_adaptor`

```cpp
template< class OuterAlloc, class... InnerAllocs >
scoped_allocator_adaptor( OuterAlloc, InnerAllocs... )
    -> scoped_allocator_adaptor<OuterAlloc, InnerAllocs...>;  // (since C++17)
```

One deduction guide is provided for `std::scoped_allocator_adaptor` to make it
possible to deduce its outer allocator.

### Example

---
*Source: https://en.cppreference.com/w/cpp/memory/scoped_allocator_adaptor/deduction_guides*
