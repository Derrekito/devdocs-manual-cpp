# Standard library header <any> (C++17)

This header is part of the general utility library.

**Classes**

- **any (C++17)** — objects that hold instances of any CopyConstructible type
  (class)
- **bad_any_cast (C++17)** — exception thrown by the value-returning forms of
  `any_cast` on a type mismatch (class)

**Functions**

- **std::swap(std::any) (C++17)** — specializes the `std::swap` algorithm
  (function)
- **make_any (C++17)** — creates an `any` object (function template)
- **any_cast (C++17)** — type-safe access to the contained object (function
  template)

### Synopsis

```cpp
namespace std {
  // class bad_any_cast
  class bad_any_cast;

  // class any
  class any;

  // non-member functions
  void swap(any& x, any& y) noexcept;

  template<class T, class... Args>
    any make_any(Args&&... args);
  template<class T, class U, class... Args>
    any make_any(initializer_list<U> il, Args&&... args);

  template<class T>
    T any_cast(const any& operand);
  template<class T>
    T any_cast(any& operand);
  template<class T>
    T any_cast(any&& operand);

  template<class T>
    const T* any_cast(const any* operand) noexcept;
  template<class T>
    T* any_cast(any* operand) noexcept;
}
```

#### Class `std::bad_any_cast`

```cpp
namespace std {
  class bad_any_cast : public bad_cast {
  public:
    // see [exception] for the specification of the special member functions
    const char* what() const noexcept override;
  };
}
```

#### Class `std::any`

```cpp
namespace std {
  class any {
  public:
    // construction and destruction
    constexpr any() noexcept;

    any(const any& other);
    any(any&& other) noexcept;

    template<class T>
      any(T&& value);

    template<class T, class... Args>
      explicit any(in_place_type_t<T>, Args&&...);
    template<class T, class U, class... Args>
      explicit any(in_place_type_t<T>, initializer_list<U>, Args&&...);

    ~any();

    // assignments
    any& operator=(const any& rhs);
    any& operator=(any&& rhs) noexcept;

    template<class T>
      any& operator=(T&& rhs);

    // modifiers
    template<class T, class... Args>
      decay_t<T>& emplace(Args&&...);
    template<class T, class U, class... Args>
      decay_t<T>& emplace(initializer_list<U>, Args&&...);
    void reset() noexcept;
    void swap(any& rhs) noexcept;

    // observers
    bool has_value() const noexcept;
    const type_info& type() const noexcept;
  };
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/any*
