# Standard library header <forward_list> (C++11)

This header is part of the containers library.

**Includes**

- **<compare> (C++20)** — Three-way comparison operator support
- **<initializer_list> (C++11)** — `std::initializer_list` class template

**Classes**

- **forward_list (C++11)** — singly-linked list (class template)

**Functions**

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (C++11)(C++11)(removed in C++20)(C++11)(removed in C++20)(C++11)(removed in
  C++20)(C++11)(removed in C++20)(C++11)(removed in C++20)(C++20)** —
  lexicographically compares the values of two `forward_lists` (function
  template)
- **std::swap(std::forward_list) (C++11)** — specializes the `std::swap`
  algorithm (function template)
- **erase(std::forward_list)erase_if(std::forward_list) (C++20)** — erases all
  elements satisfying specific criteria (function template)

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

namespace std {
  // class template forward_list
  template<class T, class Allocator = allocator<T>> class forward_list;

  template<class T, class Allocator>
    bool operator==(const forward_list<T, Allocator>& x,
                    const forward_list<T, Allocator>& y);
  template<class T, class Allocator>
    __synth_three_way_result<T> operator<=>(const forward_list<T, Allocator>& x,
                                          const forward_list<T, Allocator>& y);

  template<class T, class Allocator>
    void swap(forward_list<T, Allocator>& x, forward_list<T, Allocator>& y)
      noexcept(noexcept(x.swap(y)));

  // erasure
  template<class T, class Allocator, class U>
    typename forward_list<T, Allocator>::size_type
      erase(forward_list<T, Allocator>& c, const U& value);
  template<class T, class Allocator, class Predicate>
    typename forward_list<T, Allocator>::size_type
      erase_if(forward_list<T, Allocator>& c, Predicate pred);

  namespace pmr {
    template<class T>
      using forward_list = std::forward_list<T, polymorphic_allocator<T>>;
  }
}
```

#### Class template `std::forward_list`

```cpp
namespace std {
  template<class T, class Allocator = allocator<T>>
  class forward_list {
  public:
    // types
    using value_type      = T;
    using allocator_type  = Allocator;
    using pointer         = typename allocator_traits<Allocator>::pointer;
    using const_pointer   = typename allocator_traits<Allocator>::const_pointer;
    using reference       = value_type&;
    using const_reference = const value_type&;
    using size_type       = /* implementation-defined */;
    using difference_type = /* implementation-defined */;
    using iterator        = /* implementation-defined */;
    using const_iterator  = /* implementation-defined */;

    // construct/copy/destroy
    forward_list() : forward_list(Allocator()) { }
    explicit forward_list(const Allocator&);
    explicit forward_list(size_type n, const Allocator& = Allocator());
    forward_list(size_type n, const T& value, const Allocator& = Allocator());
    template<class InputIt>
      forward_list(InputIt first, InputIt last, const Allocator& = Allocator());
    template<container-compatible-range<T> R>
      forward_list(from_range_t, R&& rg, const Allocator& = Allocator());
    forward_list(const forward_list& x);
    forward_list(forward_list&& x);
    forward_list(const forward_list& x, const type_identity_t<Allocator>&);
    forward_list(forward_list&& x, const type_identity_t<Allocator>&);
    forward_list(initializer_list<T>, const Allocator& = Allocator());
    ~forward_list();
    forward_list& operator=(const forward_list& x);
    forward_list& operator=(forward_list&& x)
      noexcept(allocator_traits<Allocator>::is_always_equal::value);
    forward_list& operator=(initializer_list<T>);
    template<class InputIt>
      void assign(InputIt first, InputIt last);
    template<container-compatible-range<T> R>
      void assign_range(R&& rg);
    void assign(size_type n, const T& t);
    void assign(initializer_list<T>);
    allocator_type get_allocator() const noexcept;

    // iterators
    iterator before_begin() noexcept;
    const_iterator before_begin() const noexcept;
    iterator begin() noexcept;
    const_iterator begin() const noexcept;
    iterator end() noexcept;
    const_iterator end() const noexcept;

    const_iterator cbegin() const noexcept;
    const_iterator cbefore_begin() const noexcept;
    const_iterator cend() const noexcept;

    // capacity
    [[nodiscard]] bool empty() const noexcept;
    size_type max_size() const noexcept;

    // element access
    reference front();
    const_reference front() const;

    // modifiers
    template<class... Args> reference emplace_front(Args&&... args);
    void push_front(const T& x);
    void push_front(T&& x);
    template<container-compatible-range<T> R>
      void prepend_range(R&& rg);
    void pop_front();

    template<class... Args> iterator emplace_after(const_iterator position,
                                                   Args&&... args);
    iterator insert_after(const_iterator position, const T& x);
    iterator insert_after(const_iterator position, T&& x);

    iterator insert_after(const_iterator position, size_type n, const T& x);
    template<class InputIt>
      iterator insert_after(const_iterator position, InputIt first, InputIt last);
    iterator insert_after(const_iterator position, initializer_list<T> il);
    template<container-compatible-range<T> R>
      iterator insert_range_after(const_iterator position, R&& rg);

    iterator erase_after(const_iterator position);
    iterator erase_after(const_iterator position, const_iterator last);
    void swap(forward_list&)
      noexcept(allocator_traits<Allocator>::is_always_equal::value);

    void resize(size_type sz);
    void resize(size_type sz, const value_type& c);
    void clear() noexcept;

    // forward_list operations
    void splice_after(const_iterator position, forward_list& x);
    void splice_after(const_iterator position, forward_list&& x);
    void splice_after(const_iterator position, forward_list& x, const_iterator i);
    void splice_after(const_iterator position, forward_list&& x, const_iterator i);
    void splice_after(const_iterator position, forward_list& x,
                      const_iterator first, const_iterator last);
    void splice_after(const_iterator position, forward_list&& x,
                      const_iterator first, const_iterator last);

    size_type remove(const T& value);
    template<class Predicate> size_type remove_if(Predicate pred);

    size_type unique();
    template<class BinaryPredicate> size_type unique(BinaryPredicate binary_pred);

    void merge(forward_list& x);
    void merge(forward_list&& x);
    template<class Compare> void merge(forward_list& x, Compare comp);
    template<class Compare> void merge(forward_list&& x, Compare comp);

    void sort();
    template<class Compare> void sort(Compare comp);

    void reverse() noexcept;
  };

  template<class InputIt, class Allocator = allocator<__iter_value_type<InputIt>>>
    forward_list(InputIt, InputIt, Allocator = Allocator())
      -> forward_list<__iter_value_type<InputIt>, Allocator>;

  template<ranges::input_range R, class Allocator = allocator<ranges::range_value_t<R>>>
    forward_list(from_range_t, R&&, Allocator = Allocator())
      -> forward_list<ranges::range_value_t<R>, Allocator>;
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/forward_list*
