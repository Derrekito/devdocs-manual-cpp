# std::ranges::subrange<I,S,K>::size

```cpp
constexpr /* see below */ size() const
    requires (K == ranges::subrange_kind::sized);  // (since C++20)
```

Obtains the number of elements in the `subrange`.

The return type is the corresponding unsigned version of
`std::iter_difference_t<I>`.

### Parameters

(none)

### Return value

`s_ - i_` explicitly converted to the return type, where `i_` and `s_` are the
stored iterator and sentinel respectively, if the size is not stored.

Otherwise, the stored size.

### Notes

The size is stored into a `subrange` if and only if `K ==
ranges::subrange_kind::sized` but `std::sized_sentinel_for<S, I>` is not
satisfied.

### Example

### See also

- **empty (C++20)** — checks whether the `subrange` is empty (public member
  function)
- **sizessize (C++17)(C++20)** — returns the size of a container or array
  (function template)
- **ranges::size (C++20)** — returns an integer equal to the size of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/subrange/size*
