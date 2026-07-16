# std::scoped_allocator_adaptor<OuterAlloc,InnerAlloc...>::scoped_allocator_adaptor

```cpp
scoped_allocator_adaptor();  // (1) (since C++11)
template< class OuterA2 >
scoped_allocator_adaptor(
    OuterA2&& outerAlloc, const InnerAllocs&... innerAllocs
) noexcept;  // (2) (since C++11)
scoped_allocator_adaptor(
    const scoped_allocator_adaptor& other
) noexcept;  // (3) (since C++11)
scoped_allocator_adaptor(
    scoped_allocator_adaptor&& other
) noexcept;  // (4) (since C++11)
template< class OuterA2 >
scoped_allocator_adaptor(
    const scoped_allocator_adaptor<OuterA2, InnerAllocs...>& other
) noexcept;  // (5) (since C++11)
template< class OuterA2 >
scoped_allocator_adaptor(
    scoped_allocator_adaptor<OuterA2, InnerAllocs...>&& other
) noexcept;  // (6) (since C++11)
```

1) Default constructor: value-initializes the `OuterAlloc` base class and the
   inner allocator member object, if used by the implementation.

2) Constructs the base class `OuterAlloc` from
   `std::forward<OuterA2>(outerAlloc)`, and the inner allocators with
   `innerAllocs...`. This overload participates in overload resolution only if
   `std::is_constructible<OuterAlloc, OuterA2>::value` is `true`.

3) Copy-constructor: initializes each allocator from the corresponding allocator
   of `other`.

4) Move-constructor: moves each allocator from the corresponding allocator of
   `other` into `*this`.

5) Initializes each allocator from the corresponding allocator of `other`. This
   overload participates in overload resolution only if
   `std::is_constructible<OuterAlloc, const OuterA2&>::value` is `true`.

6) Initializes each allocator from the corresponding allocator of `other`, using
   move semantics. This overload participates in overload resolution only if
   `std::is_constructible<OuterAlloc, OuterA2>::value` is `true`.

### Parameters

- **outerAlloc** — constructor argument for the outer allocator
- **innerAllocs...** — constructor arguments for the inner allocators
- **other** — another `std::scoped_allocator_adaptor`

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2782 | C++11 | constructors taking `OuterA2` weren't constrained,
      interfering with metaprogramming | constraint added

### See also

- **allocate** — allocates uninitialized storage using the outer allocator
  (public member function)
- **construct** — constructs an object in allocated storage, passing the inner
  allocator to its constructor if appropriate (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/scoped_allocator_adaptor/scoped_allocator_adaptor*
