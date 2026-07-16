# Standard library header <tuple> (C++11)

This header is part of the general utility library.

**Includes**

- **<compare> (C++20)** — Three-way comparison operator support

**Classes**

- **tuple (C++11)** — implements fixed size container, which holds elements of
  possibly different types (class template)
- **tuple_size (C++11)** — obtains the number of elements of a tuple-like type
  (class template)
- **tuple_element (C++11)** — obtains the element types of a tuple-like type
  (class template)
- **std::tuple_size<std::tuple> (C++11)** — obtains the size of `tuple` at
  compile time (class template specialization)
- **std::tuple_element<std::tuple> (C++11)** — obtains the type of the specified
  element (class template specialization)
- **std::uses_allocator<std::tuple> (C++11)** — specializes the
  `std::uses_allocator` type trait (class template specialization)

**Constants**

- **ignore (C++11)** — placeholder to skip an element when unpacking a `tuple`
  using `tie` (constant)

**Functions**

- **make_tuple (C++11)** — creates a `tuple` object of the type defined by the
  argument types (function template)
- **tie (C++11)** — creates a `tuple` of lvalue references or unpacks a tuple
  into individual objects (function template)
- **forward_as_tuple (C++11)** — creates a `tuple` of forwarding references
  (function template)
- **tuple_cat (C++11)** — creates a `tuple` by concatenating any number of
  tuples (function template)
- **get(std::tuple) (C++11)** — tuple accesses specified element (function
  template)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values in
  the tuple (function template)
- **std::swap(std::tuple) (C++11)** — specializes the `std::swap` algorithm
  (function template)
- **apply (C++17)** — calls a function with a tuple of arguments (function
  template)
- **make_from_tuple (C++17)** — construct an object with a tuple of arguments
  (function template)

### Synopsis

```cpp
#include <compare>

namespace std {
  // class template tuple
  template<class... Types>
    class tuple;

  // tuple creation functions
  inline constexpr /* unspecified */ ignore;

  template<class... TTypes>
    constexpr tuple<unwrap_ref_decay_t<TTypes>...> make_tuple(TTypes&&...);

  template<class... TTypes>
    constexpr tuple<TTypes&&...> forward_as_tuple(TTypes&&...) noexcept;

  template<class... TTypes>
    constexpr tuple<TTypes&...> tie(TTypes&...) noexcept;

  template<class... Tuples>
    constexpr tuple<CTypes...> tuple_cat(Tuples&&...);

  // calling a function with a tuple of arguments
  template<class F, class Tuple>
    constexpr decltype(auto) apply(F&& f, Tuple&& t);

  template<class T, class Tuple>
    constexpr T make_from_tuple(Tuple&& t);

  // tuple helper classes
  template<class T> struct tuple_size;                  // not defined
  template<class T> struct tuple_size<const T>;

  template<class... Types> struct tuple_size<tuple<Types...>>;

  template<size_t I, class T> struct tuple_element;     // not defined
  template<size_t I, class T> struct tuple_element<I, const T>;

  template<size_t I, class... Types>
    struct tuple_element<I, tuple<Types...>>;

  template<size_t I, class T>
    using tuple_element_t = typename tuple_element<I, T>::type;

  // element access
  template<size_t I, class... Types>
    constexpr tuple_element_t<I, tuple<Types...>>& get(tuple<Types...>&) noexcept;
  template<size_t I, class... Types>
    constexpr tuple_element_t<I, tuple<Types...>>&& get(tuple<Types...>&&) noexcept;
  template<size_t I, class... Types>
    constexpr const tuple_element_t<I, tuple<Types...>>&
      get(const tuple<Types...>&) noexcept;
  template<size_t I, class... Types>
    constexpr const tuple_element_t<I, tuple<Types...>>&&
      get(const tuple<Types...>&&) noexcept;
  template<class T, class... Types>
    constexpr T& get(tuple<Types...>& t) noexcept;
  template<class T, class... Types>
    constexpr T&& get(tuple<Types...>&& t) noexcept;
  template<class T, class... Types>
    constexpr const T& get(const tuple<Types...>& t) noexcept;
  template<class T, class... Types>
    constexpr const T&& get(const tuple<Types...>&& t) noexcept;

  // relational operators
  template<class... TTypes, class... UTypes>
    constexpr bool operator==(const tuple<TTypes...>&, const tuple<UTypes...>&);
  template<class... TTypes, class... UTypes>
    constexpr common_comparison_category_t</*synth-three-way-result*/<TTypes, UTypes>...>
      operator<=>(const tuple<TTypes...>&, const tuple<UTypes...>&);

  // allocator-related traits
  template<class... Types, class Alloc>
    struct uses_allocator<tuple<Types...>, Alloc>;

  // specialized algorithms
  template<class... Types>
    constexpr void
      swap(tuple<Types...>& x, tuple<Types...>& y) noexcept(/* see description */);

  // tuple helper classes
  template<class T>
    inline constexpr size_t tuple_size_v = tuple_size<T>::value;
}

// deprecated
namespace std {
  template<class T> class tuple_size<volatile T>;
  template<class T> class tuple_size<const volatile T>;

  template<size_t I, class T> class tuple_element<I, volatile T>;
  template<size_t I, class T> class tuple_element<I, const volatile T>;
}
```

#### Class template `std::tuple`

```cpp
namespace std {
  template<class... Types>
  class tuple {
  public:
    // tuple construction
    constexpr explicit(/* see description */) tuple();
    constexpr explicit(/* see description */)
      tuple(const Types&...);         // only if sizeof...(Types) >= 1
    template<class... UTypes>
      constexpr explicit(/* see description */)
        tuple(UTypes&&...);           // only if sizeof...(Types) >= 1

    tuple(const tuple&) = default;
    tuple(tuple&&) = default;

    template<class... UTypes>
      constexpr explicit(/* see description */) tuple(const tuple<UTypes...>&);
    template<class... UTypes>
      constexpr explicit(/* see description */) tuple(tuple<UTypes...>&&);

    template<class U1, class U2>
      constexpr explicit(/* see description */)
        tuple(const pair<U1, U2>&);   // only if sizeof...(Types) == 2
    template<class U1, class U2>
      constexpr explicit(/* see description */)
        tuple(pair<U1, U2>&&);        // only if sizeof...(Types) == 2

    // allocator-extended constructors
    template<class Alloc>
      constexpr explicit(/* see description */)
        tuple(allocator_arg_t, const Alloc& a);
    template<class Alloc>
      constexpr explicit(/* see description */)
        tuple(allocator_arg_t, const Alloc& a, const Types&...);
    template<class Alloc, class... UTypes>
      constexpr explicit(/* see description */)
        tuple(allocator_arg_t, const Alloc& a, UTypes&&...);
    template<class Alloc>
      constexpr tuple(allocator_arg_t, const Alloc& a, const tuple&);
    template<class Alloc>
      constexpr tuple(allocator_arg_t, const Alloc& a, tuple&&);
    template<class Alloc, class... UTypes>
      constexpr explicit(/* see description */)
        tuple(allocator_arg_t, const Alloc& a, const tuple<UTypes...>&);
    template<class Alloc, class... UTypes>
      constexpr explicit(/* see description */)
        tuple(allocator_arg_t, const Alloc& a, tuple<UTypes...>&&);
    template<class Alloc, class U1, class U2>
      constexpr explicit(/* see description */)
        tuple(allocator_arg_t, const Alloc& a, const pair<U1, U2>&);
    template<class Alloc, class U1, class U2>
      constexpr explicit(/* see description */)
        tuple(allocator_arg_t, const Alloc& a, pair<U1, U2>&&);

    // tuple assignment
    constexpr tuple& operator=(const tuple&);
    constexpr tuple& operator=(tuple&&) noexcept(/* see description */);

    template<class... UTypes>
      constexpr tuple& operator=(const tuple<UTypes...>&);
    template<class... UTypes>
      constexpr tuple& operator=(tuple<UTypes...>&&);

    template<class U1, class U2>
      constexpr tuple& operator=(const pair<U1, U2>&); // only if sizeof...(Types) == 2
    template<class U1, class U2>
      constexpr tuple& operator=(pair<U1, U2>&&);      // only if sizeof...(Types) == 2

    // tuple swap
    constexpr void swap(tuple&) noexcept(/* see description */);
  };

  template<class... UTypes>
    tuple(UTypes...) -> tuple<UTypes...>;
  template<class T1, class T2>
    tuple(pair<T1, T2>) -> tuple<T1, T2>;
  template<class Alloc, class... UTypes>
    tuple(allocator_arg_t, Alloc, UTypes...) -> tuple<UTypes...>;
  template<class Alloc, class T1, class T2>
    tuple(allocator_arg_t, Alloc, pair<T1, T2>) -> tuple<T1, T2>;
  template<class Alloc, class... UTypes>
    tuple(allocator_arg_t, Alloc, tuple<UTypes...>) -> tuple<UTypes...>;
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/tuple*
