# std::pair

```cpp
template<
    class T1,
    class T2
> struct pair;
```

`std::pair` is a class template that provides a way to store two heterogeneous
objects as a single unit. A pair is a specific case of a `std::tuple` with two
elements.

If neither `T1` nor `T2` is a possibly cv-qualified class type with non-trivial
destructor, or array thereof, the destructor of `pair` is trivial.

### Template parameters

- **T1, T2** — the types of the elements that the pair stores.

### Member types

- **`first_type`** — `T1`
- **`second_type`** — `T2`

### Member objects

- **`first`** — `T1`
- **`second`** — `T2`

### Member functions

- **(constructor)** — constructs new pair (public member function)
- **operator=** — assigns the contents (public member function)
- **swap (C++11)** — swaps the contents (public member function)

### Non-member functions

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

### Helper classes

- **std::tuple_size<std::pair> (C++11)** — obtains the size of a `pair` (class
  template specialization)
- **std::tuple_element<std::pair> (C++11)** — obtains the type of the elements
  of `pair` (class template specialization)
- **std::basic_common_reference<std::pair> (C++23)** — determines the common
  reference type of two `pair`s (class template specialization)
- **std::common_type<std::pair> (C++23)** — determines the common type of two
  `pair`s (class template specialization)
- **std::formatter<*pair-or-tuple*> (C++23)** — formatting support for `pair`
  and `tuple` (class template specialization)

### Deduction guides(since C++17)

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2796 | C++98 | triviality of the destructor of `pair` was unspecified |
      specified

### See also

- **tuple (C++11)** — implements fixed size container, which holds elements of
  possibly different types (class template)
- **tie (C++11)** — creates a `tuple` of lvalue references or unpacks a tuple
  into individual objects (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/pair*
