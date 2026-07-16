# std::in_place, std::in_place_type, std::in_place_index, std::in_place_t, std::in_place_type_t, std::in_place_index_t

```cpp
struct in_place_t { explicit in_place_t() = default; };  // (1) (since C++17)
inline constexpr std::in_place_t in_place {};  // (2) (since C++17)
template< class T >
struct in_place_type_t { explicit in_place_type_t() = default; };  // (3) (since C++17)
template< class T >
inline constexpr std::in_place_type_t<T> in_place_type {};  // (4) (since C++17)
template< std::size_t I >
struct in_place_index_t { explicit in_place_index_t() = default; };  // (5) (since C++17)
template< std::size_t I >
inline constexpr std::in_place_index_t<I> in_place_index {};  // (6) (since C++17)
```

1,3,5) The type/type templates `std::in_place_t`, `std::in_place_type_t` and
   `std::in_place_index_t` can be used in the constructor's parameter list to
   match the intended tag.

2,4,6) The corresponding `std::in_place`, `std::in_place_type`, and
   `std::in_place_index` instances of (1,3,5) are disambiguation tags that can
   be passed to the constructors of `std::expected`, `std::optional`,
   `std::variant`, and `std::any` to indicate that the contained object should
   be constructed in-place, and (for the latter two) the type of the object to
   be constructed.

### See also

- **expected (C++23)** — a wrapper that contains either an expected or error
  value (class template)
- **optional (C++17)** — a wrapper that may or may not hold an object (class
  template)
- **variant (C++17)** — a type-safe discriminated union (class template)
- **any (C++17)** — objects that hold instances of any CopyConstructible type
  (class)

---
*Source: https://en.cppreference.com/w/cpp/utility/in_place*
