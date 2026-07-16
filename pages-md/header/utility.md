# Standard library header <utility>

This header is part of the general utility library.

**Includes**

- **<compare> (C++20)** — Three-way comparison operator support
- **<initializer_list> (C++11)** — `std::initializer_list` class template

**Namespaces**

- **`rel_ops`** — Provide automatic comparison operators
- **operator!=operator>operator<=operator>= (deprecated in C++20)** —
  automatically generates comparison operators based on user-defined
  `operator==` and `operator<` (function template)

**Functions**

- **swap** — swaps the values of two objects (function template)
- **exchange (C++14)** — replaces the argument with a new value and returns its
  previous value (function template)
- **forward (C++11)** — forwards a function argument (function template)
- **forward_like (C++23)** — forwards a function argument as if casting it to
  the value category and constness of the expression of specified type template
  argument (function template)
- **move (C++11)** — obtains an rvalue reference (function template)
- **move_if_noexcept (C++11)** — obtains an rvalue reference if the move
  constructor does not throw (function template)
- **as_const (C++17)** — obtains a reference to const to its argument (function
  template)
- **declval (C++11)** — obtains a reference to its argument for use in
  unevaluated context (function template)
- **to_underlying (C++23)** — converts an enumeration to its underlying type
  (function template)
- **cmp_equalcmp_not_equalcmp_lesscmp_greatercmp_less_equalcmp_greater_equal
  (C++20)** — compares two integer values without value change caused by
  conversion (function template)
- **in_range (C++20)** — checks if an integer value is in the range of a given
  integer type (function template)
- **unreachable (C++23)** — marks unreachable point of execution (function)
- **make_pair** — creates a `pair` object of type, defined by the argument types
  (function template)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values in
  the pair (function template)
- **std::swap(std::pair) (C++11)** — specializes the `std::swap` algorithm
  (function template)
- **get(std::pair) (C++11)** — accesses an element of a `pair` (function
  template)

**Classes**

- **pair** — implements binary tuple, i.e. a pair of values (class template)
- **tuple_size (C++11)** — obtains the number of elements of a tuple-like type
  (class template)
- **tuple_element (C++11)** — obtains the element types of a tuple-like type
  (class template)
- **std::tuple_size<std::pair> (C++11)** — obtains the size of a `pair` (class
  template specialization)
- **std::tuple_element<std::pair> (C++11)** — obtains the type of the elements
  of `pair` (class template specialization)
- **integer_sequence (C++14)** — implements compile-time sequence of integers
  (class template)

**Forward declarations**

- **tuple (C++11)** — implements fixed size container, which holds elements of
  possibly different types (class template)

**Tags**

- **piecewise_constructpiecewise_construct_t (C++11)** — piecewise construction
  tag (tag)
- **in_placein_place_typein_place_indexin_place_tin_place_type_tin_place_index_t
  (C++17)** — in-place construction tag (tag)
- **nontype nontype_t (C++26)** — value construction tag (tag)

### Synopsis

```cpp
#include <compare>
#include <initializer_list>

namespace std {
  // swap
  template<class T>
    constexpr void swap(T& a, T& b) noexcept(/* see description */);
  template<class T, size_t N>
    constexpr void swap(T (&a)[N], T (&b)[N]) noexcept(is_nothrow_swappable_v<T>);

  // exchange
  template<class T, class U = T>
    constexpr T exchange(T& obj, U&& new_val);

  // forward/move
  template<class T>
    constexpr T&& forward(remove_reference_t<T>& t) noexcept;
  template<class T>
    constexpr T&& forward(remove_reference_t<T>&& t) noexcept;
  template<class T, class U>
    constexpr /* see description */ forward_like(U&& x) noexcept;
  template<class T>
    constexpr remove_reference_t<T>&& move(T&&) noexcept;
  template<class T>
    constexpr conditional_t<
        !is_nothrow_move_constructible_v<T> && is_copy_constructible_v<T>, const T&, T&&>
      move_if_noexcept(T& x) noexcept;

  // as_const
  template<class T>
    constexpr add_const_t<T>& as_const(T& t) noexcept;
  template<class T>
    void as_const(const T&&) = delete;

  // declval
  template<class T>
    add_rvalue_reference_t<T> declval() noexcept;   // as unevaluated operand

  // integer comparison functions
  template<class T, class U>
    constexpr bool cmp_equal(T t, U u) noexcept;
  template<class T, class U>
    constexpr bool cmp_not_equal(T t, U u) noexcept;

  template<class T, class U>
    constexpr bool cmp_less(T t, U u) noexcept;
  template<class T, class U>
    constexpr bool cmp_greater(T t, U u) noexcept;
  template<class T, class U>
    constexpr bool cmp_less_equal(T t, U u) noexcept;
  template<class T, class U>
    constexpr bool cmp_greater_equal(T t, U u) noexcept;

  template<class R, class T>
    constexpr bool in_range(T t) noexcept;

  // to_underlying
  template<class T>
    constexpr underlying_type_t<T> to_underlying(T value) noexcept;

  // unreachable
  [[noreturn]] void unreachable();

  // compile-time integer sequences
  template<class T, T...>
    struct integer_sequence;
  template<size_t... I>
    using index_sequence = integer_sequence<size_t, I...>;

  template<class T, T N>
    using make_integer_sequence = integer_sequence<T, /* see description */>;
  template<size_t N>
    using make_index_sequence = make_integer_sequence<size_t, N>;

  template<class... T>
    using index_sequence_for = make_index_sequence<sizeof...(T)>;

  // class template pair
  template<class T1, class T2>
    struct pair;

  // pair specialized algorithms
  template<class T1, class T2>
    constexpr bool operator==(const pair<T1, T2>&, const pair<T1, T2>&);
  template<class T1, class T2>
    constexpr common_comparison_category_t</*synth-three-way-result*/<T1>,
                                           /*synth-three-way-result*/<T2>>
      operator<=>(const pair<T1, T2>&, const pair<T1, T2>&);

  template<class T1, class T2>
    constexpr void swap(pair<T1, T2>& x, pair<T1, T2>& y) noexcept(noexcept(x.swap(y)));

  template<class T1, class T2>
    constexpr /* see description */ make_pair(T1&&, T2&&);

  // tuple-like access to pair
  template<class T> struct tuple_size;
  template<size_t I, class T> struct tuple_element;

  template<class T1, class T2> struct tuple_size<pair<T1, T2>>;
  template<size_t I, class T1, class T2> struct tuple_element<I, pair<T1, T2>>;

  template<size_t I, class T1, class T2>
    constexpr tuple_element_t<I, pair<T1, T2>>& get(pair<T1, T2>&) noexcept;
  template<size_t I, class T1, class T2>
    constexpr tuple_element_t<I, pair<T1, T2>>&& get(pair<T1, T2>&&) noexcept;
  template<size_t I, class T1, class T2>
    constexpr const tuple_element_t<I, pair<T1, T2>>& get(const pair<T1, T2>&) noexcept;
  template<size_t I, class T1, class T2>
    constexpr const tuple_element_t<I, pair<T1, T2>>&& get(const pair<T1, T2>&&) noexcept;
  template<class T1, class T2>
    constexpr T1& get(pair<T1, T2>& p) noexcept;
  template<class T1, class T2>
    constexpr const T1& get(const pair<T1, T2>& p) noexcept;
  template<class T1, class T2>
    constexpr T1&& get(pair<T1, T2>&& p) noexcept;
  template<class T1, class T2>
    constexpr const T1&& get(const pair<T1, T2>&& p) noexcept;
  template<class T2, class T1>
    constexpr T2& get(pair<T1, T2>& p) noexcept;
  template<class T2, class T1>
    constexpr const T2& get(const pair<T1, T2>& p) noexcept;
  template<class T2, class T1>
    constexpr T2&& get(pair<T1, T2>&& p) noexcept;
  template<class T2, class T1>
    constexpr const T2&& get(const pair<T1, T2>&& p) noexcept;

  // pair piecewise construction
  struct piecewise_construct_t {
    explicit piecewise_construct_t() = default;
  };
  inline constexpr piecewise_construct_t piecewise_construct{};
  template<class... Types> class tuple;         // defined in <tuple>

  // in-place construction
  struct in_place_t {
    explicit in_place_t() = default;
  };
  inline constexpr in_place_t in_place{};

  template<class T>
    struct in_place_type_t {
      explicit in_place_type_t() = default;
    };
  template<class T> inline constexpr in_place_type_t<T> in_place_type{};

  template<size_t I>
    struct in_place_index_t {
      explicit in_place_index_t() = default;
    };
  template<size_t I> inline constexpr in_place_index_t<I> in_place_index{};

  // nontype argument tag
  template<auto V>
    struct nontype_t {
      explicit nontype_t() = default;
    };
  template<auto V> inline constexpr nontype_t<V> nontype{};
}

// deprecated
namespace std::rel_ops {
  template<class T> bool operator!=(const T&, const T&);
  template<class T> bool operator> (const T&, const T&);
  template<class T> bool operator<=(const T&, const T&);
  template<class T> bool operator>=(const T&, const T&);
}
```

#### Class template `std::integer_sequence`

```cpp
namespace std {
  template<class T, T... I> struct integer_sequence {
    using value_type = T;
    static constexpr size_t size() noexcept { return sizeof...(I); }
  };
}
```

#### Class template `std::pair`

```cpp
namespace std {
  template<class T1, class T2>
  struct pair {
    using first_type  = T1;
    using second_type = T2;

    T1 first;
    T2 second;

    pair(const pair&) = default;
    pair(pair&&) = default;
    constexpr explicit(/* see description */) pair();
    constexpr explicit(/* see description */) pair(const T1& x, const T2& y);
    template<class U1 = T1, class U2 = T2>
      constexpr explicit(/* see description */) pair(U1&& x, U2&& y);
    template<class U1, class U2>
      constexpr explicit(/* see description */) pair(const pair<U1, U2>& p);
    template<class U1, class U2>
      constexpr explicit(/* see description */) pair(pair<U1, U2>&& p);
    template<class... Args1, class... Args2>
      constexpr pair(piecewise_construct_t,
                     tuple<Args1...> first_args, tuple<Args2...> second_args);

    constexpr pair& operator=(const pair& p);
    template<class U1, class U2>
      constexpr pair& operator=(const pair<U1, U2>& p);
    constexpr pair& operator=(pair&& p) noexcept(/* see description */);
    template<class U1, class U2>
      constexpr pair& operator=(pair<U1, U2>&& p);

    constexpr void swap(pair& p) noexcept(/* see description */);
  };

  template<class T1, class T2>
    pair(T1, T2) -> pair<T1, T2>;
}
```

### See also

- **<tuple> (C++11)** — `std::tuple` class template

---
*Source: https://en.cppreference.com/w/cpp/header/utility*
