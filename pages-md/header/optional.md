# Standard library header <optional> (C++17)

**Includes**

- **<compare> (C++20)** — Three-way comparison operator support

**Classes**

- **optional (C++17)** — a wrapper that may or may not hold an object (class
  template)
- **bad_optional_access (C++17)** — exception indicating checked access to an
  optional that doesn't contain a value (class)
- **std::hash<std::optional> (C++17)** — hash support for `std::optional` (class
  template specialization)
- **nullopt_t (C++17)** — indicator of optional type with uninitialized state
  (class)

**Forward declarations**

- **hash (C++11)** — hash function object (class template)

**Constants**

- **nullopt (C++17)** — an object of type `nullopt_t` (constant)

**Functions**

**Comparison**

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (C++17)(C++17)(C++17)(C++17)(C++17)(C++17)(C++20)** — compares `optional`
  objects (function template)

**Specialized algorithms**

- **std::swap(std::optional) (C++17)** — specializes the `std::swap` algorithm
  (function template)
- **make_optional (C++17)** — creates an `optional` object (function template)

### Synopsis

```cpp
#include <compare>

namespace std {
  // class template optional
  template<class T>
    class optional;

  // no-value state indicator
  struct nullopt_t{/* see description */};
  inline constexpr nullopt_t nullopt(/* unspecified */);

  // class bad_optional_access
  class bad_optional_access;

  // relational operators
  template<class T, class U>
    constexpr bool operator==(const optional<T>&, const optional<U>&);
  template<class T, class U>
    constexpr bool operator!=(const optional<T>&, const optional<U>&);
  template<class T, class U>
    constexpr bool operator<(const optional<T>&, const optional<U>&);
  template<class T, class U>
    constexpr bool operator>(const optional<T>&, const optional<U>&);
  template<class T, class U>
    constexpr bool operator<=(const optional<T>&, const optional<U>&);
  template<class T, class U>
    constexpr bool operator>=(const optional<T>&, const optional<U>&);
  template<class T, three_way_comparable_with<T> U>
    constexpr compare_three_way_result_t<T,U>
      operator<=>(const optional<T>&, const optional<U>&);

  // comparison with nullopt
  template<class T> constexpr bool operator==(const optional<T>&, nullopt_t) noexcept;
  template<class T>
    constexpr strong_ordering operator<=>(const optional<T>&, nullopt_t) noexcept;

  // comparison with T
  template<class T, class U> constexpr bool operator==(const optional<T>&, const U&);
  template<class T, class U> constexpr bool operator==(const T&, const optional<U>&);
  template<class T, class U> constexpr bool operator!=(const optional<T>&, const U&);
  template<class T, class U> constexpr bool operator!=(const T&, const optional<U>&);
  template<class T, class U> constexpr bool operator<(const optional<T>&, const U&);
  template<class T, class U> constexpr bool operator<(const T&, const optional<U>&);
  template<class T, class U> constexpr bool operator>(const optional<T>&, const U&);
  template<class T, class U> constexpr bool operator>(const T&, const optional<U>&);
  template<class T, class U> constexpr bool operator<=(const optional<T>&, const U&);
  template<class T, class U> constexpr bool operator<=(const T&, const optional<U>&);
  template<class T, class U> constexpr bool operator>=(const optional<T>&, const U&);
  template<class T, class U> constexpr bool operator>=(const T&, const optional<U>&);
  template<class T, three_way_comparable_with<T> U>
    constexpr compare_three_way_result_t<T,U>
      operator<=>(const optional<T>&, const U&);

  // specialized algorithms
  template<class T>
    constexpr void swap(optional<T>&, optional<T>&) noexcept(/* see description */);

  template<class T>
    constexpr optional</* see description */> make_optional(T&&);
  template<class T, class... Args>
    constexpr optional<T> make_optional(Args&&... args);
  template<class T, class U, class... Args>
    constexpr optional<T> make_optional(initializer_list<U> il, Args&&... args);

  // hash support
  template<class T> struct hash;
  template<class T> struct hash<optional<T>>;
}
```

#### Class template `std::optional`

```cpp
namespace std {
  template<class T>
  class optional {
  public:
    using value_type = T;

    // constructors
    constexpr optional() noexcept;
    constexpr optional(nullopt_t) noexcept;
    constexpr optional(const optional&);
    constexpr optional(optional&&) noexcept(/* see description */);
    template<class... Args>
      constexpr explicit optional(in_place_t, Args&&...);
    template<class U, class... Args>
      constexpr explicit optional(in_place_t, initializer_list<U>, Args&&...);
    template<class U = T>
      constexpr explicit(/* see description */) optional(U&&);
    template<class U>
      constexpr explicit(/* see description */) optional(const optional<U>&);
    template<class U>
      constexpr explicit(/* see description */) optional(optional<U>&&);

    // destructor
    constexpr ~optional();

    // assignment
    constexpr optional& operator=(nullopt_t) noexcept;
    constexpr optional& operator=(const optional&);
    constexpr optional& operator=(optional&&) noexcept(/* see description */);
    template<class U = T> constexpr optional& operator=(U&&);
    template<class U> constexpr optional& operator=(const optional<U>&);
    template<class U> constexpr optional& operator=(optional<U>&&);
    template<class... Args> constexpr T& emplace(Args&&...);
    template<class U, class... Args> constexpr T& emplace(initializer_list<U>, Args&&...);

    // swap
    constexpr void swap(optional&) noexcept(/* see description */);

    // observers
    constexpr const T* operator->() const noexcept;
    constexpr T* operator->() noexcept;
    constexpr const T& operator*() const& noexcept;
    constexpr T& operator*() & noexcept;
    constexpr T&& operator*() && noexcept;
    constexpr const T&& operator*() const&& noexcept;
    constexpr explicit operator bool() const noexcept;
    constexpr bool has_value() const noexcept;
    constexpr const T& value() const&;
    constexpr T& value() &;
    constexpr T&& value() &&;
    constexpr const T&& value() const&&;
    template<class U> constexpr T value_or(U&&) const&;
    template<class U> constexpr T value_or(U&&) &&;

    // monadic operations
    template <class F> constexpr auto and_then(F&& f) &;
    template <class F> constexpr auto and_then(F&& f) &&;
    template <class F> constexpr auto and_then(F&& f) const&;
    template <class F> constexpr auto and_then(F&& f) const&&;
    template <class F> constexpr auto transform(F&& f) &;
    template <class F> constexpr auto transform(F&& f) &&;
    template <class F> constexpr auto transform(F&& f) const&;
    template <class F> constexpr auto transform(F&& f) const&&;
    template <class F> constexpr optional or_else(F&& f) &&;
    template <class F> constexpr optional or_else(F&& f) const&;

    // modifiers
    constexpr void reset() noexcept;

  private:
    T *val;         // exposition only
  };

  template<class T>
    optional(T) -> optional<T>;
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/optional*
