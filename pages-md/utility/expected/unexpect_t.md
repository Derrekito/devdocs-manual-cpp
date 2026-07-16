# std::unexpect_t, std::unexpect

```cpp
struct unexpect_t {
    explicit unexpect_t() = default;
};  // (1) (since C++23)
inline constexpr unexpect_t unexpect{};  // (2) (since C++23)
```

1) A tag type for in-place construction of an unexpected value in an
   `std::expected` object.

2) A constant of type `const std::unexpect_t` which is usually directly passed
   to a constructor of `std::expected` to construct an unexpected value.

### Notes

Like other construction tag types, `unexpect_t` is a trivial, empty class with
an explicit default constructor.

### See also

- **(constructor)** — constructs the `expected` object (public member function)
- **in_placein_place_typein_place_indexin_place_tin_place_type_tin_place_index_t
  (C++17)** — in-place construction tag (tag)

---
*Source: https://en.cppreference.com/w/cpp/utility/expected/unexpect_t*
