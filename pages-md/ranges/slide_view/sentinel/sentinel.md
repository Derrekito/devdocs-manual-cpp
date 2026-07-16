# std::ranges::slide_view<V>::*sentinel*::*sentinel*

```cpp
/*sentinel*/() = default;  // (1) (since C++23)
private:
constexpr /*sentinel*/( ranges::sentinel_t<V> end );  // (2) (exposition only*)
```

Constructs a sentinel.

1) Default constructor. Value-initializes the underlying sentinel `end_` with
   `ranges::sentinel_t<V>()`.

2) A private constructor which is used by `slide_view::end`. This constructor is
   not accessible to users. Initializes `end_` with `end`.

### Parameters

- **end** — a sentinel

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/slide_view/sentinel/sentinel*
