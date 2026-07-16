# std::common_type<*tuple-like*>

```cpp
template< tuple-like TTuple, tuple-like UTuple >
    requires /* see below */
struct common_type<TTuple, UTuple>;  // (since C++23)
```

The common type of two `tuple-like` types is a `std::tuple` consists of the
common types of all corresponding element type pairs of both types.

Given

- `TTypes` as the pack formed by the sequence of std::tuple_element_t<i, TTuple>
  for every integer `i` in `[``​0``,``std::tuple_size_v<TTuple>``)`, and
- `UTypes` as the pack formed by the sequence of std::tuple_element_t<i, UTuple>
  for every integer `i` in `[``​0``,``std::tuple_size_v<UTuple>``)`,

the following constraints need to be satisfied:

- `TTuple` or `UTuple` is a `std::tuple` specialization.
- `std::is_same_v<TTuple, std::decay_t<TTuple>>` is `true`.
- `std::is_same_v<UTuple, std::decay_t<UTuple>>` is `true`.
- `std::tuple_size_v<TTuple>` equals `std::tuple_size_v<UTuple>` (`TTuple` and
  `UTuple` have the same number of elements).
- std::tuple<std::common_type_t<TTypes, UTypes>...> denotes a type.

### Member types

- **`type`** — std::tuple<std::common_type_t<TTypes, UTypes>...>

### Example

### See also

- **common_type (C++11)** — determines the common type of a group of types
  (class template)
- **std::common_type<std::pair> (C++23)** — determines the common type of two
  `pair`s (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/utility/tuple/common_type*
