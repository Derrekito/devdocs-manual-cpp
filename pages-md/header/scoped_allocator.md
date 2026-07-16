# Standard library header <scoped_allocator> (C++11)

This header is part of the dynamic memory management library.

**Classes**

- **scoped_allocator_adaptor (C++11)** — implements multi-level allocator for
  multi-level containers (class template)

**Functions**

- **operator==operator!= (removed in C++20)** — compares two
  scoped_allocator_adaptor instances (function template)

### Synopsis

```cpp
namespace std {
  // class template scoped_allocator_adaptor
  template<class OuterAlloc, class... InnerAlloc>
    class scoped_allocator_adaptor;

  // scoped allocator operators
  template<class OuterA1, class OuterA2, class... InnerAllocs>
    bool operator==(const scoped_allocator_adaptor<OuterA1, InnerAllocs...>& a,
                    const scoped_allocator_adaptor<OuterA2, InnerAllocs...>& b) noexcept;
}
```

#### Class template `std::scoped_allocator_adaptor`

```cpp
namespace std {
  template<class OuterAlloc, class... InnerAllocs>
  class scoped_allocator_adaptor : public OuterAlloc {
  private:
    using OuterTraits = allocator_traits<OuterAlloc>;   // exposition only
    scoped_allocator_adaptor<InnerAllocs...> inner;     // exposition only

  public:
    using outer_allocator_type = OuterAlloc;
    using inner_allocator_type = /* see description */;

    using value_type           = typename OuterTraits::value_type;
    using size_type            = typename OuterTraits::size_type;
    using difference_type      = typename OuterTraits::difference_type;
    using pointer              = typename OuterTraits::pointer;
    using const_pointer        = typename OuterTraits::const_pointer;
    using void_pointer         = typename OuterTraits::void_pointer;
    using const_void_pointer   = typename OuterTraits::const_void_pointer;

    using propagate_on_container_copy_assignment = /* see description */;
    using propagate_on_container_move_assignment = /* see description */;
    using propagate_on_container_swap            = /* see description */;
    using is_always_equal                        = /* see description */;

    template<class Tp> struct rebind {
      using other = scoped_allocator_adaptor<
        OuterTraits::template rebind_alloc<Tp>, InnerAllocs...>;
    };

    scoped_allocator_adaptor();
    template<class OuterA2>
      scoped_allocator_adaptor(OuterA2&& outerAlloc,
                               const InnerAllocs&... innerAllocs) noexcept;

    scoped_allocator_adaptor(const scoped_allocator_adaptor& other) noexcept;
    scoped_allocator_adaptor(scoped_allocator_adaptor&& other) noexcept;

    template<class OuterA2>
      scoped_allocator_adaptor(
        const scoped_allocator_adaptor<OuterA2, InnerAllocs...>& other) noexcept;
    template<class OuterA2>
      scoped_allocator_adaptor(
        scoped_allocator_adaptor<OuterA2, InnerAllocs...>&& other) noexcept;

    scoped_allocator_adaptor& operator=(const scoped_allocator_adaptor&) = default;
    scoped_allocator_adaptor& operator=(scoped_allocator_adaptor&&) = default;

    ~scoped_allocator_adaptor();

    inner_allocator_type& inner_allocator() noexcept;
    const inner_allocator_type& inner_allocator() const noexcept;
    outer_allocator_type& outer_allocator() noexcept;
    const outer_allocator_type& outer_allocator() const noexcept;

    [[nodiscard]] pointer allocate(size_type n);
    [[nodiscard]] pointer allocate(size_type n, const_void_pointer hint);
    void deallocate(pointer p, size_type n);
    size_type max_size() const;

    template<class T, class... Args>
      void construct(T* p, Args&&... args);

    template<class T>
      void destroy(T* p);

    scoped_allocator_adaptor select_on_container_copy_construction() const;
  };

  template<class OuterAlloc, class... InnerAllocs>
    scoped_allocator_adaptor(OuterAlloc, InnerAllocs...)
      -> scoped_allocator_adaptor<OuterAlloc, InnerAllocs...>;
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/scoped_allocator*
