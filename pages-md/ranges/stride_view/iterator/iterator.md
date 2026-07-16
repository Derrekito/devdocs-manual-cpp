# std::ranges::stride_view<V>::*iterator*<Const>::*iterator*

```cpp
/*iterator*/()
    requires std::default_initializable<ranges::iterator_t<Base>> = default;  // (1) (since C++23)
constexpr /*iterator*/( /*iterator*/<!Const> i )
    requires Const and
        std::convertible_to<ranges::iterator_t<V>, ranges::iterator_t<Base>> and
        std::convertible_to<ranges::sentinel_t<V>, ranges::sentinel_t<Base>>;  // (2) (since C++23)
private:
constexpr /*iterator*/( /*Parent*/& parent,
                        ranges::iterator_t<Base> current,
                        ranges::range_difference_t<Base> missing = 0 );  // (3) (exposition only*)
```

Constructs an iterator.

1) Default constructor. Value-initializes:

- `current_` with `ranges::iterator_t<Base>()`,
- `end_` with `ranges::sentinel_t<Base>()`,
- `stride_` with `​0​`,
- `missing_` with `​0​`.

2) Conversion from `/*iterator*/<false>` to `/*iterator*/<true>`. Initializes:

- `current_` with `std::move(i.current_)`,
- `end_` with `std::move(i.end_)`,
- `stride_` with `i.stride_`,
- `missing_` with `i.missing_`.

3) A private constructor which is used by `stride_view::begin` and
`stride_view::end`. This constructor is not accessible to users. Initializes

- `current_` with `std::move(current)`,
- `end_` with `ranges::end(parent->base_)`,
- `stride_` with `parent->stride_`,
- `missing_` with `missing`.

### Parameters

- **i** — an `/*iterator*/<false>`

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/stride_view/iterator/iterator*
