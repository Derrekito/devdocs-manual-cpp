# Standard library header <stacktrace> (C++23)

This header is part of the general utility library.

**Classes**

- **stacktrace_entry (C++23)** — representation of an evaluation in a stacktrace
  (class)
- **basic_stacktrace (C++23)** — approximate representation of an invocation
  sequence consists of stacktrace entries (class template)
- **std::hash<std::stacktrace_entry> (C++23)** — hash support for
  `std::stacktrace_entry` (class template specialization)
- **std::hash<std::basic_stacktrace> (C++23)** — hash support for
  `std::basic_stacktrace` (class template specialization)

**Forward declarations**

- **hash (C++11)** — hash function object (class template)

**Type aliases**

- **`std::stacktrace`** —
  std::basic_stacktrace<std::allocator<std::stacktrace_entry>>
- **`std::pmr::stacktrace`** — std::pmr::basic_stacktrace<
  std::pmr::polymorphic_allocator<std::stacktrace_entry>>

**Functions**

- **std::swap(std::basic_stacktrace) (C++23)** — specializes the `std::swap`
  algorithm (function template)
- **to_string (C++23)** — returns a string with a description of the
  `stacktrace_entry` (function)
- **to_string (C++23)** — returns a string with a description of the
  `basic_stacktrace` (function template)
- **operator<< (C++23)** — performs stream output of `stacktrace_entry`
  (function template)
- **operator<< (C++23)** — performs stream output of `basic_stracktrace`
  (function template)

### Synopsis

```cpp
namespace std {
  // class stacktrace_entry
  class stacktrace_entry;

  // class template basic_stacktrace
  template<class Allocator>
    class basic_stacktrace;

  // basic_stacktrace typedef names
  using stacktrace = basic_stacktrace<allocator<stacktrace_entry>>;

  // non-member functions
  template<class Allocator>
    void swap(basic_stacktrace<Allocator>& a, basic_stacktrace<Allocator>& b)
      noexcept(noexcept(a.swap(b)));

  string to_string(const stacktrace_entry& f);

  template<class Allocator>
    string to_string(const basic_stacktrace<Allocator>& st);

  ostream& operator<<(ostream& os, const stacktrace_entry& f);

  template<class Allocator>
    ostream& operator<<(ostream& os, const basic_stacktrace<Allocator>& st);

  namespace pmr {
    using stacktrace = std::basic_stacktrace<polymorphic_allocator<stacktrace_entry>>;
  }

  // hash support
  template<class T> struct hash;
  template<> struct hash<stacktrace_entry>;
  template<class Allocator> struct hash<basic_stacktrace<Allocator>>;
}
```

#### Class `std::stacktrace_entry`

```cpp
namespace std {
  class stacktrace_entry {
  public:
    using native_handle_type = /* implementation-defined */;

    // constructors
    constexpr stacktrace_entry() noexcept;
    constexpr stacktrace_entry(const stacktrace_entry& other) noexcept;
    constexpr stacktrace_entry& operator=(const stacktrace_entry& other) noexcept;

    ~stacktrace_entry();

    // observers
    constexpr native_handle_type native_handle() const noexcept;
    constexpr explicit operator bool() const noexcept;

    // query
    string description() const;
    string source_file() const;
    uint_least32_t source_line() const;

    // comparison
    friend constexpr bool operator==(const stacktrace_entry& x,
                                     const stacktrace_entry& y) noexcept;
    friend constexpr strong_ordering operator<=>(const stacktrace_entry& x,
                                                 const stacktrace_entry& y) noexcept;
  };
}
```

#### Class template `std::basic_stacktrace`

```cpp
namespace std {
  template<class Allocator>
  class basic_stacktrace {
  public:
    using value_type = stacktrace_entry;
    using const_reference = const value_type&;
    using reference = value_type&;
    using const_iterator = /* implementation-defined */;
    using iterator = const_iterator;
    using reverse_iterator = std::reverse_iterator<iterator>;
    using const_reverse_iterator = std::reverse_iterator<const_iterator>;
    using difference_type = /* implementation-defined */;
    using size_type = /* implementation-defined */;
    using allocator_type = Allocator;

    // creation and assignment
    static basic_stacktrace
      current(const allocator_type& alloc = allocator_type()) noexcept;
    static basic_stacktrace
      current(size_type skip, const allocator_type& alloc = allocator_type()) noexcept;
    static basic_stacktrace
      current(size_type skip, size_type max_depth,
              const allocator_type& alloc = allocator_type()) noexcept;

    basic_stacktrace() noexcept(is_nothrow_default_constructible_v<allocator_type>);
    explicit basic_stacktrace(const allocator_type& alloc) noexcept;

    basic_stacktrace(const basic_stacktrace& other);
    basic_stacktrace(basic_stacktrace&& other) noexcept;
    basic_stacktrace(const basic_stacktrace& other, const allocator_type& alloc);
    basic_stacktrace(basic_stacktrace&& other, const allocator_type& alloc);
    basic_stacktrace& operator=(const basic_stacktrace& other);
    basic_stacktrace& operator=(basic_stacktrace&& other) noexcept(
        allocator_traits<Allocator>::propagate_on_container_move_assignment::value ||
        allocator_traits<Allocator>::is_always_equal::value);

    ~basic_stacktrace();

    // observers
    allocator_type get_allocator() const noexcept;

    const_iterator begin() const noexcept;
    const_iterator end() const noexcept;
    const_reverse_iterator rbegin() const noexcept;
    const_reverse_iterator rend() const noexcept;

    const_iterator cbegin() const noexcept;
    const_iterator cend() const noexcept;
    const_reverse_iterator crbegin() const noexcept;
    const_reverse_iterator crend() const noexcept;

    [[nodiscard]] bool empty() const noexcept;
    size_type size() const noexcept;
    size_type max_size() const noexcept;

    const_reference operator[](size_type) const;
    const_reference at(size_type) const;

    // comparisons
    template<class Allocator2>
    friend bool operator==(const basic_stacktrace& x,
                           const basic_stacktrace<Allocator2>& y) noexcept;
    template<class Allocator2>
    friend strong_ordering operator<=>(const basic_stacktrace& x,
                                       const basic_stacktrace<Allocator2>& y) noexcept;

    // modifiers
    void swap(basic_stacktrace& other)
      noexcept(allocator_traits<Allocator>::propagate_on_container_swap::value ||
        allocator_traits<Allocator>::is_always_equal::value);

  private:
    vector<value_type, allocator_type> frames_;         // exposition only
  };
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/stacktrace*
