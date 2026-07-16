# std::chrono::local_info

```cpp
struct local_info;  // (since C++20)
```

The class `local_info` describes the result of converting a
`std::chrono::local_time` to a `std::chrono::sys_time`.

- If the result of the conversion is unique, then `result ==
  local_info::unique`, `first` is filled out with the correct
  `std::chrono::sys_info`, and `second` is zero-initialized.
- If the `local_time` is nonexistent, then `result == local_info::nonexistent`,
  `first` is filled out with the `std::chrono::sys_info` that ends just prior to
  the `local_time`, and `second` is filled out with the `std::chrono::sys_info`
  that begins just after the `local_time`.
- If the `local_time` is ambiguous, then `result == local_info::ambiguous`,
  `first` is filled out with the `std::chrono::sys_info` that ends just after
  the `local_time`, and `second` is filled with the `std::chrono::sys_info` that
  starts just before the `local_time`.

This is a low-level data structure; typical conversions from `local_time` to
`sys_time` will use it implicitly rather than explicitly.

### Member constants

- **constexpr int unique [static]** — `​0​` (public static member constant)
- **constexpr int nonexistent [static]** — `1` (public static member constant)
- **constexpr int ambiguous [static]** — `2` (public static member constant)

### Member objects

- **`result`** — int
- **`first`, `second`** — `std::chrono::sys_info`

### Nonmember functions

- **operator<< (C++20)** — outputs a `local_info` into a stream (function
  template)

### Helper classes

- **std::formatter<std::chrono::local_info> (C++20)** — formatting support for
  `local_info` (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/chrono/local_info*
