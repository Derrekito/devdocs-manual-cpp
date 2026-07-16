# Standard library header <variant> (C++17)

This header is part of the general utility library.

**Includes**

- **<compare> (C++20)** — Three-way comparison operator support

**Classes**

- **variant (C++17)** — a type-safe discriminated union (class template)
- **monostate (C++17)** — placeholder type for use as the first alternative in a
  variant of non-default-constructible types (class)
- **bad_variant_access (C++17)** — exception thrown on invalid accesses to the
  value of a variant (class)
- **variant_sizevariant_size_v (C++17)** — obtains the size of the variant's
  list of alternatives at compile time (class template) (variable template)
- **variant_alternativevariant_alternative_t (C++17)** — obtains the type of the
  alternative specified by its index, at compile time (class template) (alias
  template)
- **std::hash<std::variant> (C++17)** — hash support for `std::variant` (class
  template specialization)

**Constants**

- **variant_npos (C++17)** — index of the variant in the invalid state
  (constant)

**Functions**

- **visit (C++17)** — calls the provided functor with the arguments held by one
  or more variants (function template)
- **holds_alternative (C++17)** — checks if a variant currently holds a given
  type (function template)
- **get(std::variant) (C++17)** — reads the value of the variant given the index
  or the type (if the type is unique), throws on error (function template)
- **get_if (C++17)** — obtains a pointer to the value of a pointed-to variant
  given the index or the type (if unique), returns null on error (function
  template)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (C++17)(C++17)(C++17)(C++17)(C++17)(C++17)(C++20)** — compares `variant`
  objects as their contained values (function template)
- **std::swap(std::variant) (C++17)** — specializes the `std::swap` algorithm
  (function template)

### Synopsis

```cpp
#include <compare>

namespace std {
  // class template variant
  template<class... Types>
    class variant;

  // variant helper classes
  template<class T> struct variant_size;                        // not defined
  template<class T> struct variant_size<const T>;
  template<class T>
    inline constexpr size_t variant_size_v = variant_size<T>::value;

  template<class... Types>
    struct variant_size<variant<Types...>>;

  template<size_t I, class T> struct variant_alternative;       // not defined
  template<size_t I, class T> struct variant_alternative<I, const T>;
  template<size_t I, class T>
    using variant_alternative_t = typename variant_alternative<I, T>::type;

  template<size_t I, class... Types>
    struct variant_alternative<I, variant<Types...>>;

  inline constexpr size_t variant_npos = -1;

  // value access
  template<class T, class... Types>
    constexpr bool holds_alternative(const variant<Types...>&) noexcept;

  template<size_t I, class... Types>
    constexpr variant_alternative_t<I, variant<Types...>>& get(variant<Types...>&);
  template<size_t I, class... Types>
    constexpr variant_alternative_t<I, variant<Types...>>&& get(variant<Types...>&&);
  template<size_t I, class... Types>
    constexpr const variant_alternative_t<I, variant<Types...>>&
      get(const variant<Types...>&);
  template<size_t I, class... Types>
    constexpr const variant_alternative_t<I, variant<Types...>>&&
      get(const variant<Types...>&&);

  template<class T, class... Types>
    constexpr T& get(variant<Types...>&);
  template<class T, class... Types>
    constexpr T&& get(variant<Types...>&&);
  template<class T, class... Types>
    constexpr const T& get(const variant<Types...>&);
  template<class T, class... Types>
    constexpr const T&& get(const variant<Types...>&&);

  template<size_t I, class... Types>
    constexpr add_pointer_t<variant_alternative_t<I, variant<Types...>>>
      get_if(variant<Types...>*) noexcept;
  template<size_t I, class... Types>
    constexpr add_pointer_t<const variant_alternative_t<I, variant<Types...>>>
      get_if(const variant<Types...>*) noexcept;

  template<class T, class... Types>
    constexpr add_pointer_t<T>
      get_if(variant<Types...>*) noexcept;
  template<class T, class... Types>
    constexpr add_pointer_t<const T>
      get_if(const variant<Types...>*) noexcept;

  // relational operators
  template<class... Types>
    constexpr bool operator==(const variant<Types...>&, const variant<Types...>&);
  template<class... Types>
    constexpr bool operator!=(const variant<Types...>&, const variant<Types...>&);
  template<class... Types>
    constexpr bool operator<(const variant<Types...>&, const variant<Types...>&);
  template<class... Types>
    constexpr bool operator>(const variant<Types...>&, const variant<Types...>&);
  template<class... Types>
    constexpr bool operator<=(const variant<Types...>&, const variant<Types...>&);
  template<class... Types>
    constexpr bool operator>=(const variant<Types...>&, const variant<Types...>&);
  template<class... Types> requires (three_way_comparable<Types> && ...)
    constexpr common_comparison_category_t<compare_three_way_result_t<Types>...>
      operator<=>(const variant<Types...>&, const variant<Types...>&);

  // visitation
  template<class Visitor, class... Variants>
    constexpr /* see description */ visit(Visitor&&, Variants&&...);
  template<class R, class Visitor, class... Variants>
    constexpr R visit(Visitor&&, Variants&&...);

  // class monostate
  struct monostate;

  // monostate relational operators
  constexpr bool operator==(monostate, monostate) noexcept;
  constexpr strong_ordering operator<=>(monostate, monostate) noexcept;

  // specialized algorithms
  template<class... Types>
    constexpr void swap(variant<Types...>&,
                        variant<Types...>&) noexcept(/* see description */);

  // class bad_variant_access
  class bad_variant_access;

  // hash support
  template<class T> struct hash;
  template<class... Types> struct hash<variant<Types...>>;
  template<> struct hash<monostate>;
}

// deprecated
namespace std {
  template<class T> struct variant_size<volatile T>;
  template<class T> struct variant_size<const volatile T>;

  template<size_t I, class T> struct variant_alternative<I, volatile T>;
  template<size_t I, class T> struct variant_alternative<I, const volatile T>;
}
```

#### Class template `std::variant`

```cpp
namespace std {
  template<class... Types>
  class variant {
  public:
    // constructors
    constexpr variant() noexcept(/* see description */);
    constexpr variant(const variant&);
    constexpr variant(variant&&) noexcept(/* see description */);

    template<class T>
      constexpr variant(T&&) noexcept(/* see description */);

    template<class T, class... Args>
      constexpr explicit variant(in_place_type_t<T>, Args&&...);
    template<class T, class U, class... Args>
      constexpr explicit variant(in_place_type_t<T>, initializer_list<U>, Args&&...);

    template<size_t I, class... Args>
      constexpr explicit variant(in_place_index_t<I>, Args&&...);
    template<size_t I, class U, class... Args>
      constexpr explicit variant(in_place_index_t<I>, initializer_list<U>, Args&&...);

    // destructor
    constexpr ~variant();

    // assignment
    constexpr variant& operator=(const variant&);
    constexpr variant& operator=(variant&&) noexcept(/* see description */);

    template<class T> constexpr variant& operator=(T&&) noexcept(/* see description */);

    // modifiers
    template<class T, class... Args>
      constexpr T& emplace(Args&&...);
    template<class T, class U, class... Args>
      constexpr T& emplace(initializer_list<U>, Args&&...);
    template<size_t I, class... Args>
      constexpr variant_alternative_t<I, variant<Types...>>& emplace(Args&&...);
    template<size_t I, class U, class... Args>
      constexpr variant_alternative_t<I, variant<Types...>>&
        emplace(initializer_list<U>, Args&&...);

    // value status
    constexpr bool valueless_by_exception() const noexcept;
    constexpr size_t index() const noexcept;

    // swap
    constexpr void swap(variant&) noexcept(/* see description */);
  };
}
```

#### Class `std::monostate`

```cpp
struct monostate{};
```

#### Class `std::bad_variant_access`

```cpp
class bad_variant_access : public exception {
public:
  // see [exception] for the specification of the special member functions
  const char* what() const noexcept override;
};
```

---
*Source: https://en.cppreference.com/w/cpp/header/variant*
