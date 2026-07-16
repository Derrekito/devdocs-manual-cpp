# std::ranges::chunk_view<V>::*iterator*<Const>::*iterator*

```cpp
/*iterator*/() = default;  // (1) (since C++23)
constexpr /*iterator*/( /*iterator*/<!Const> i )
    requires
        Const and
        std::convertible_to<ranges::iterator_t<V>, ranges::iterator_t<Base>> and
        std::convertible_to<ranges::sentinel_t<V>, ranges::sentinel_t<Base>>;  // (2) (since C++23)
private:
constexpr /*iterator*/( Parent* parent,
                        ranges::iterator_t<Base> current,
                        ranges::range_difference_t<Base> missing = 0 );  // (3) (exposition only*)
```

Construct an iterator.

1) Default constructor. Value-initializes the underlying data members:

- `current_` with `ranges::iterator_t<Base>()`,
- `end_` with `ranges::sentinel_t<Base>()`,
- `n_` with `​0​`,
- `missing_` with `​0​`.

2) Conversion from `/*iterator*/<false>` to `/*iterator*/<true>`. Initializes
the underlying data members:

- `current_` with `std::move(i.current_)`,
- `end_` with `std::move(i.end_)`,
- `n_` with `i.n_`,
- `missing_` with `i.missing_`.

3) A private constructor which is used by `ranges::chunk_view::begin` and
`ranges::chunk_view::end`. This constructor is not accessible to users.
Initializes the underlying data members:

- `current_` with `current`,
- `end_` with `ranges::end(parent->base_)`,
- `n_` with `parent->n_`,
- `missing_` with `missing`.

### Parameters

- **i** — an `/*iterator*/<false>`
- **parent** — a pointer to owning `chunk_view`
- **current** — an iterator to the begin of current chunk
- **missing** — a difference between expected (`n_`) and actual size of current
  chunk

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/iterator/iterator*
