# std::ranges::borrowed_iterator_t, std::ranges::borrowed_subrange_t

```cpp
template< ranges::range R >
using borrowed_iterator_t = /* see below */;  // (1) (since C++20)
template< ranges::range R >
using borrowed_subrange_t = /* see below */;  // (2) (since C++20)
```

1) `std::ranges::iterator_t<R>` if `R` models `borrowed_range`,
   `std::ranges::dangling` otherwise.

2) `std::ranges::subrange<std::ranges::iterator_t<R>>` if `R` models
   `borrowed_range`, `std::ranges::dangling` otherwise.

These two alias templates are used by some constrained algorithms to avoid
returning potentially dangling iterators or views.

### Possible implementation

```cpp
template< std::ranges::range R >
using borrowed_iterator_t = std::conditional_t<std::ranges::borrowed_range<R>,
    std::ranges::iterator_t<R>, std::ranges::dangling>;
```

```cpp
template< std::ranges::range R >
using borrowed_subrange_t = std::conditional_t<std::ranges::borrowed_range<R>,
    std::ranges::subrange<std::ranges::iterator_t<R>>, std::ranges::dangling>;
```

### See also

- **ranges::dangling (C++20)** — a placeholder type indicating that an iterator
  or a `subrange` should not be returned since it would be dangling (class)

---
*Source: https://en.cppreference.com/w/cpp/ranges/borrowed_iterator_t*
