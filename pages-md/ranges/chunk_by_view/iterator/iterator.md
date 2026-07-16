# std::ranges::chunk_by_view<V,Pred>::*iterator*::*iterator*

```cpp
/*iterator*/() = default;  // (1) (since C++23)
private:
constexpr /*iterator*/( chunk_by_view& parent,
                        ranges::iterator_t<V> current,
                        ranges::iterator_t<V> next );  // (2) (exposition only*)
```

Construct an iterator.

1) Default constructor. Value-initializes the underlying data-members as
follows:

- `parent_` with nullptr,
- `current_` with `ranges::iterator_t<V>()`,
- `next_` with `ranges::iterator_t<V>()`.

2) A private constructor which is used by `chunk_by_view::begin` and
`chunk_by_view::end`. This constructor is not accessible to users. Initializes:

- `parent_` with `std::addressof(parent)`,
- `current_` with `current`,
- `next_` with `next`.

### Parameters

- **parent** — a parent object
- **current, next** — iterators

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_by_view/iterator/iterator*
