# Standard library header <list>

This header is part of the containers library.

**Includes**

- **<compare> (C++20)** — Three-way comparison operator support
- **<initializer_list> (C++11)** — `std::initializer_list` class template

**Classes**

- **list** — doubly-linked list (class template)

**Functions**

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values of
  two `lists` (function template)
- **std::swap(std::list)** — specializes the `std::swap` algorithm (function
  template)
- **erase(std::list)erase_if(std::list) (C++20)** — erases all elements
  satisfying specific criteria (function template)

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
  // class template list
  template<class T, class Allocator = allocator<T>> class list;

  template<class T, class Allocator>
    bool operator==(const list<T, Allocator>& x, const list<T, Allocator>& y);
  template<class T, class Allocator>
    __synth_three_way_result<T> operator<=>(const list<T, Allocator>& x,
                                          const list<T, Allocator>& y);

  template<class T, class Allocator>
    void swap(list<T, Allocator>& x, list<T, Allocator>& y)
      noexcept(noexcept(x.swap(y)));

  // erasure
  template<class T, class Allocator, class U>
    typename list<T, Allocator>::size_type
      erase(list<T, Allocator>& c, const U& value);
  template<class T, class Allocator, class Predicate>
    typename list<T, Allocator>::size_type
      erase_if(list<T, Allocator>& c, Predicate pred);

  namespace pmr {
    template<class T>
      using list = std::list<T, polymorphic_allocator<T>>;
  }
}
```

#### Class template `std::list`

```cpp
namespace std {
  template<class T, class Allocator = allocator<T>>
  class list {
  public:
    // types
    using value_type             = T;
    using allocator_type         = Allocator;
    using pointer                = typename allocator_traits<Allocator>::pointer;
    using const_pointer          = typename allocator_traits<Allocator>::const_pointer;
    using reference              = value_type&;
    using const_reference        = const value_type&;
    using size_type              = /* implementation-defined */;
    using difference_type        = /* implementation-defined */;
    using iterator               = /* implementation-defined */;
    using const_iterator         = /* implementation-defined */;
    using reverse_iterator       = std::reverse_iterator<iterator>;
    using const_reverse_iterator = std::reverse_iterator<const_iterator>;

    // construct/copy/destroy
    list() : list(Allocator()) { }
    explicit list(const Allocator&);
    explicit list(size_type n, const Allocator& = Allocator());
    list(size_type n, const T& value, const Allocator& = Allocator());
    template<class InputIt>
      list(InputIt first, InputIt last, const Allocator& = Allocator());
    template<container-compatible-range<T> R>
      list(from_range_t, R&& rg, const Allocator& = Allocator());
    list(const list& x);
    list(list&& x);
    list(const list&, const type_identity_t<Allocator>&);
    list(list&&, const type_identity_t<Allocator>&);
    list(initializer_list<T>, const Allocator& = Allocator());
    ~list();
    list& operator=(const list& x);
    list& operator=(list&& x)
      noexcept(allocator_traits<Allocator>::is_always_equal::value);
    list& operator=(initializer_list<T>);
    template<class InputIt>
      void assign(InputIt first, InputIt last);
    template<container-compatible-range<T> R>
      void assign_range(R&& rg);
    void assign(size_type n, const T& t);
    void assign(initializer_list<T>);
    allocator_type get_allocator() const noexcept;

    // iterators
    iterator               begin() noexcept;
    const_iterator         begin() const noexcept;
    iterator               end() noexcept;
    const_iterator         end() const noexcept;
    reverse_iterator       rbegin() noexcept;
    const_reverse_iterator rbegin() const noexcept;
    reverse_iterator       rend() noexcept;
    const_reverse_iterator rend() const noexcept;

    const_iterator         cbegin() const noexcept;
    const_iterator         cend() const noexcept;
    const_reverse_iterator crbegin() const noexcept;
    const_reverse_iterator crend() const noexcept;

    // capacity
    [[nodiscard]] bool empty() const noexcept;
    size_type size() const noexcept;
    size_type max_size() const noexcept;
    void      resize(size_type sz);
    void      resize(size_type sz, const T& c);

    // element access
    reference       front();
    const_reference front() const;
    reference       back();
    const_reference back() const;

    // modifiers
    template<class... Args> reference emplace_front(Args&&... args);
    template<class... Args> reference emplace_back(Args&&... args);
    void push_front(const T& x);
    void push_front(T&& x);
    template<container-compatible-range<T> R>
      void prepend_range(R&& rg);
    void pop_front();
    void push_back(const T& x);
    void push_back(T&& x);
    template<container-compatible-range<T> R>
      void append_range(R&& rg);
    void pop_back();

    template<class... Args> iterator emplace(const_iterator position, Args&&... args);
    iterator insert(const_iterator position, const T& x);
    iterator insert(const_iterator position, T&& x);
    iterator insert(const_iterator position, size_type n, const T& x);
    template<class InputIt>
      iterator insert(const_iterator position, InputIt first, InputIt last);
    template<container-compatible-range<T> R>
      iterator insert_range(const_iterator position, R&& rg);
    iterator insert(const_iterator position, initializer_list<T> il);

    iterator erase(const_iterator position);
    iterator erase(const_iterator position, const_iterator last);
    void     swap(list&) noexcept(allocator_traits<Allocator>::is_always_equal::value);
    void     clear() noexcept;

    // list operations
    void splice(const_iterator position, list& x);
    void splice(const_iterator position, list&& x);
    void splice(const_iterator position, list& x, const_iterator i);
    void splice(const_iterator position, list&& x, const_iterator i);
    void splice(const_iterator position, list& x,
                const_iterator first, const_iterator last);
    void splice(const_iterator position, list&& x,
                const_iterator first, const_iterator last);

    size_type remove(const T& value);
    template<class Predicate> size_type remove_if(Predicate pred);

    size_type unique();
    template<class BinaryPredicate>
      size_type unique(BinaryPredicate binary_pred);

    void merge(list& x);
    void merge(list&& x);
    template<class Compare> void merge(list& x, Compare comp);
    template<class Compare> void merge(list&& x, Compare comp);

    void sort();
    template<class Compare> void sort(Compare comp);

    void reverse() noexcept;
  };

  template<class InputIt, class Allocator = allocator<__iter_value_type<InputIt>>>
    list(InputIt, InputIt, Allocator = Allocator())
      -> list<__iter_value_type<InputIt>, Allocator>;

  template<ranges::input_range R, class Allocator = allocator<ranges::range_value_t<R>>>
    list(from_range_t, R&&, Allocator = Allocator())
      -> list<ranges::range_value_t<R>, Allocator>;
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/list*
