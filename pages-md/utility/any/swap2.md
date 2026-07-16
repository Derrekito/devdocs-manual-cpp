# std::swap(std::any)

```cpp
void swap( any& lhs, any& rhs ) noexcept;  // (since C++17)
```

Overloads the `std::swap` algorithm for `std::any`. Swaps the content of two
`any` objects by calling `lhs.swap(rhs)`.

### Parameters

- **lhs, rhs** — objects to swap

### Return value

(none)

### See also

- **swap** — swaps two `any` objects (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/any/swap2*
