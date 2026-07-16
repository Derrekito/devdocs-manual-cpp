# Standard library header <array> (C++11)

This header is part of the containers library.

**Includes**

- **<compare> (C++20)** — Three-way comparison operator support
- **<initializer_list> (C++11)** — `std::initializer_list` class template

**Classes**

- **array (C++11)** — static contiguous array (class template)
- **tuple_size (C++11)** — obtains the number of elements of a tuple-like type
  (class template)
- **tuple_element (C++11)** — obtains the element types of a tuple-like type
  (class template)
- **std::tuple_size<std::array> (C++11)** — obtains the size of an `array`
  (class template specialization)
- **std::tuple_element<std::array> (C++11)** — obtains the type of the elements
  of `array` (class template specialization)

**Functions**

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (C++11)(C++11)(removed in C++20)(C++11)(removed in C++20)(C++11)(removed in
  C++20)(C++11)(removed in C++20)(C++11)(removed in C++20)(C++20)** —
  lexicographically compares the values of two `arrays` (function template)
- **std::swap(std::array) (C++11)** — specializes the `std::swap` algorithm
  (function template)
- **to_array (C++20)** — creates a `std::array` object from a built-in array
  (function template)
- **get(std::array) (C++11)** — accesses an element of an `array` (function
  template)

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
#include <compare>
#include <initializer_list>

namespace std
{
    // class template array
    template<class T, size_t N> struct array;

    template<class T, size_t N>
        constexpr bool operator==(const array<T, N>& x, const array<T, N>& y);
    template<class T, size_t N>
        constexpr __synth_three_way_result<T>
            operator<=>(const array<T, N>& x, const array<T, N>& y);

    // specialized algorithms
    template<class T, size_t N>
        constexpr void swap(array<T, N>& x, array<T, N>& y) noexcept(noexcept(x.swap(y)));

    // array creation functions
    template<class T, size_t N>
        constexpr array<remove_cv_t<T>, N> to_array(T (&a)[N]);
    template<class T, size_t N>
        constexpr array<remove_cv_t<T>, N> to_array(T (&&a)[N]);

    // tuple interface
    template<class T> struct tuple_size;
    template<size_t I, class T> struct tuple_element;
    template<class T, size_t N>
        struct tuple_size<array<T, N>>;
    template<size_t I, class T, size_t N>
        struct tuple_element<I, array<T, N>>;
    template<size_t I, class T, size_t N>
        constexpr T& get(array<T, N>&) noexcept;
    template<size_t I, class T, size_t N>
        constexpr T&& get(array<T, N>&&) noexcept;
    template<size_t I, class T, size_t N>
        constexpr const T& get(const array<T, N>&) noexcept;
    template<size_t I, class T, size_t N>
        constexpr const T&& get(const array<T, N>&&) noexcept;
}
```

#### Class template `std::array`

```cpp
namespace std
{
    template<class T, size_t N>
    struct array
    {
        // types
        using value_type             = T;
        using pointer                = T*;
        using const_pointer          = const T*;
        using reference              = T&;
        using const_reference        = const T&;
        using size_type              = size_t;
        using difference_type        = ptrdiff_t;
        using iterator               = /* implementation-defined */;
        using const_iterator         = /* implementation-defined */;
        using reverse_iterator       = std::reverse_iterator<iterator>;
        using const_reverse_iterator = std::reverse_iterator<const_iterator>;

        // no explicit construct/copy/destroy for aggregate type

        constexpr void fill(const T& u);
        constexpr void swap(array&) noexcept(is_nothrow_swappable_v<T>);

        // iterators
        constexpr iterator               begin() noexcept;
        constexpr const_iterator         begin() const noexcept;
        constexpr iterator               end() noexcept;
        constexpr const_iterator         end() const noexcept;

        constexpr reverse_iterator       rbegin() noexcept;
        constexpr const_reverse_iterator rbegin() const noexcept;
        constexpr reverse_iterator       rend() noexcept;
        constexpr const_reverse_iterator rend() const noexcept;

        constexpr const_iterator         cbegin() const noexcept;
        constexpr const_iterator         cend() const noexcept;
        constexpr const_reverse_iterator crbegin() const noexcept;
        constexpr const_reverse_iterator crend() const noexcept;

        // capacity
        [[nodiscard]] constexpr bool empty() const noexcept;
        constexpr size_type size() const noexcept;
        constexpr size_type max_size() const noexcept;

        // element access
        constexpr reference       operator[](size_type n);
        constexpr const_reference operator[](size_type n) const;
        constexpr reference       at(size_type n);
        constexpr const_reference at(size_type n) const;
        constexpr reference       front();
        constexpr const_reference front() const;
        constexpr reference       back();
        constexpr const_reference back() const;

        constexpr T*       data() noexcept;
        constexpr const T* data() const noexcept;
    };

    template<class T, class... U>
        array(T, U...) -> array<T, 1 + sizeof...(U)>;
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/array*
