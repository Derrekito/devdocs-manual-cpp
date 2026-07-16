# std::swap(std::mdspan)

```cpp
friend constexpr void swap( mdspan& x, mdspan& y ) noexcept;  // (since C++23)
```

Overloads the `std::swap` algorithm for `std::mdspan`. Exchanges the state of
`x` with that of `y`.

Given `ptr_`, `map_`, and `acc_` as exposition-only data members, equivalent to:

```cpp
std::swap(x.ptr_, y.ptr_);
std::swap(x.map_, y.map_);
std::swap(x.acc_, y.acc_);
```

### Parameters

- **x, y** — `mdspan` objects whose states to swap

### Return value

(none)

### Example

### See also

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/swap2*
