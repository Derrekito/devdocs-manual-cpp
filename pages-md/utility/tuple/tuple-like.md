# *tuple-like*, *pair-like*

```cpp
template< class T >
concept tuple-like = /* see below */;  // (1) (since C++23) (exposition only*)
template< class T >
concept pair-like =
    tuple-like<T> && std::tuple_size_v<std::remove_cvref_t<T>> == 2;  // (2) (since C++23) (exposition only*)
```

1) A type `T` models and satisfies the concept `tuple-like` if
std::remove_cvref_t<T> is a specialization of

- `std::array`,

- `std::complex`,
*(since C++26)*

- `std::pair`,
- `std::tuple`, or
- `std::ranges::subrange`.

2) `pair-like` objects are `tuple-like` objects with exactly 2 elements.

### Notes

`tuple-like` types implement the *tuple protocol*, i.e., such types can be used
with `std::get`, `std::tuple_element` and `std::tuple_size`.

Elements of `tuple-like` types can be bound with structured binding.

### See also

`tuple-like` and `pair-like` are used in the following standard library
components:

- **(constructor)** — constructs a new `tuple` (public member function)
- **operator=** — assigns the contents of one `tuple` to another (public member
  function)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values in
  the tuple (function template)
- **std::basic_common_reference<*tuple-like*> (C++23)** — determines the common
  reference type of a `tuple` and a `tuple-like` type (class template
  specialization)
- **std::common_type<*tuple-like*> (C++23)** — determines the common type of a
  `tuple` and a `tuple-like` type (class template specialization)
- **tuple_cat (C++11)** — creates a `tuple` by concatenating any number of
  tuples (function template)
- **apply (C++17)** — calls a function with a tuple of arguments (function
  template)
- **make_from_tuple (C++17)** — construct an object with a tuple of arguments
  (function template)
- **(constructor)** — constructs new pair (public member function of
  `std::pair<T1,T2>`)
- **operator=** — assigns the contents (public member function of
  `std::pair<T1,T2>`)
- **operator PairLike (C++20)** — converts the `subrange` to a **`pair-like`**
  type (public member function of `std::ranges::subrange<I,S,K>`)
- **ranges::elements_viewviews::elements (C++20)** — takes a `view` consisting
  of **`tuple-like`** values and a number N and produces a `view` of Nth element
  of each tuple (class template) (range adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/utility/tuple/tuple-like*
