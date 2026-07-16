# std::ranges::subrange_kind

```cpp
enum class subrange_kind : bool {
    unsized,
    sized
};  // (since C++20)
```

Specifies if a `std::ranges::subrange` models `std::ranges::sized_range` or not.

- **`unsized`** — specifies that the `subrange` does not model `sized_range`
- **`sized`** — specifies that the `subrange` does model `sized_range`

---
*Source: https://en.cppreference.com/w/cpp/ranges/subrange_kind*
