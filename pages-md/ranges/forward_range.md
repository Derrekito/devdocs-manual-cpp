# std::ranges::forward_range

```cpp
template< class T >
  concept forward_range =
    ranges::input_range<T> && std::forward_iterator<ranges::iterator_t<T>>;  // (since C++20)
```

The `forward_range` concept is a refinement of `range` for which `ranges::begin`
returns a model of `forward_iterator`.

---
*Source: https://en.cppreference.com/w/cpp/ranges/forward_range*
