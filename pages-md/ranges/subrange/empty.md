# std::ranges::subrange<I,S,K>::empty

```cpp
constexpr bool empty() const;  // (since C++20)
```

Checks whether the `subrange` is empty, i.e. the stored iterator and sentinel
compare equal.

### Parameters

(none)

### Return value

`true` if the stored iterator and sentinel compare equal, `false` otherwise.

### Example

### See also

- **size (C++20)** — obtains the size of the `subrange` (public member function)
- **empty (C++17)** — checks whether the container is empty (function template)
- **ranges::empty (C++20)** — checks whether a range is empty (customization
  point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/subrange/empty*
