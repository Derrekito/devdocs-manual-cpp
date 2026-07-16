# Standard library header <span> (C++20)

This header is part of the containers library.

**Classes**

- **span (C++20)** — a non-owning view over a contiguous sequence of objects
  (class template)

**Constants**

- **dynamic_extent (C++20)** — a constant of type `std::size_t` signifying that
  the `span` has dynamic extent (constant)

**Functions**

- **as_bytesas_writable_bytes (C++20)** — converts a `span` into a view of its
  underlying bytes (function template)

**Range access**

- **begincbegin (C++11)(C++14)** — returns an iterator to the beginning of a
  container or array (function template)
- **endcend (C++11)(C++14)** — returns an iterator to the end of a container or
  array (function template)
- **rbegincrbegin (C++14)** — returns a reverse iterator to the beginning of a
  container or array (function template)
- **rendcrend (C++14)** — returns a reverse end iterator for a container or
  array (function template)
- **sizessize (C++17)(C++20)** — returns the size of a container or array
  (function template)
- **empty (C++17)** — checks whether the container is empty (function template)
- **data (C++17)** — obtains the pointer to the underlying array (function
  template)

### Synopsis

```cpp
namespace std {
  // constants
  inline constexpr size_t dynamic_extent = numeric_limits<size_t>::max();

  // class template span
  template<class ElementType, size_t Extent = dynamic_extent>
    class span;

  template<class ElementType, size_t Extent>
    constexpr bool ranges::enable_view<span<ElementType, Extent>> = true;
  template<class ElementType, size_t Extent>
    constexpr bool ranges::enable_borrowed_range<span<ElementType, Extent>> = true;

  // views of object representation
  template<class ElementType, size_t Extent>
    span<const byte,
         Extent == dynamic_extent ? dynamic_extent : sizeof(ElementType) * Extent>
      as_bytes(span<ElementType, Extent> s) noexcept;

  template<class ElementType, size_t Extent>
    span<byte, Extent == dynamic_extent ? dynamic_extent : sizeof(ElementType) * Extent>
      as_writable_bytes(span<ElementType, Extent> s) noexcept;
}
```

#### Class template `std::span`

```cpp
namespace std {
  template<class ElementType, size_t Extent = dynamic_extent>
  class span {
  public:
    // constants and types
    using element_type = ElementType;
    using value_type = remove_cv_t<ElementType>;
    using size_type = size_t;
    using difference_type = ptrdiff_t;
    using pointer = element_type*;
    using const_pointer = const element_type*;
    using reference = element_type&;
    using const_reference = const element_type&;
    using iterator = /* implementation-defined */;
    using const_iterator = std::const_iterator<iterator>;
    using reverse_iterator = std::reverse_iterator<iterator>;
    using const_reverse_iterator = std::const_iterator<reverse_iterator>;
    static constexpr size_type extent = Extent;

    // constructors, copy, and assignment
    constexpr span() noexcept;
    template<class It>
      constexpr explicit(extent != dynamic_extent) span(It first, size_type count);
    template<class It, class End>
      constexpr explicit(extent != dynamic_extent) span(It first, End last);
    template<size_t N>
      constexpr span(type_identity_t<element_type> (&arr)[N]) noexcept;
    template<class T, size_t N>
      constexpr span(array<T, N>& arr) noexcept;
    template<class T, size_t N>
      constexpr span(const array<T, N>& arr) noexcept;
    template<class R>
      constexpr explicit(extent != dynamic_extent) span(R&& r);
    constexpr span(const span& other) noexcept = default;
    template<class OtherElementType, size_t OtherExtent>
      constexpr explicit(/* see description */)
        span(const span<OtherElementType, OtherExtent>& s) noexcept;

    constexpr span& operator=(const span& other) noexcept = default;

    // subviews
    template<size_t Count>
      constexpr span<element_type, Count> first() const;
    template<size_t Count>
      constexpr span<element_type, Count> last() const;
    template<size_t Offset, size_t Count = dynamic_extent>
      constexpr span<element_type, /* see description */> subspan() const;

    constexpr span<element_type, dynamic_extent> first(size_type count) const;
    constexpr span<element_type, dynamic_extent> last(size_type count) const;
    constexpr span<element_type, dynamic_extent> subspan(
      size_type offset, size_type count = dynamic_extent) const;

    // observers
    constexpr size_type size() const noexcept;
    constexpr size_type size_bytes() const noexcept;
    [[nodiscard]] constexpr bool empty() const noexcept;

    // element access
    constexpr reference operator[](size_type idx) const;
    constexpr reference front() const;
    constexpr reference back() const;
    constexpr pointer data() const noexcept;

    // iterator support
    constexpr iterator begin() const noexcept;
    constexpr iterator end() const noexcept;
    constexpr const_iterator cbegin() const noexcept { return begin(); }
    constexpr const_iterator cend() const noexcept { return end(); }
    constexpr reverse_iterator rbegin() const noexcept;
    constexpr reverse_iterator rend() const noexcept;
    constexpr const_reverse_iterator crbegin() const noexcept { return rbegin(); }
    constexpr const_reverse_iterator crend() const noexcept { return rend(); }

  private:
    pointer data_;   // exposition only
    size_type size_; // exposition only
  };

  template<class It, class EndOrSize>
    span(It, EndOrSize) -> span<remove_reference_t<iter_reference_t<It>>>;
  template<class T, size_t N>
    span(T (&)[N]) -> span<T, N>;
  template<class T, size_t N>
    span(array<T, N>&) -> span<T, N>;
  template<class T, size_t N>
    span(const array<T, N>&) -> span<const T, N>;
  template<class R>
    span(R&&) -> span<remove_reference_t<ranges::range_reference_t<R>>>;
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/span*
