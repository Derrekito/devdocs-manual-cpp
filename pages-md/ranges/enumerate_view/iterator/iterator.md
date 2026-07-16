# std::ranges::enumerate_view<V>::*iterator*<Const>::*iterator*

```cpp
/*iterator*/()
    requires std::default_initializable<ranges::iterator_t<Base>> = default;  // (1) (since C++23)
constexpr /*iterator*/( /*iterator*/<!Const> i )
    requires Const &&
        std::convertible_to<ranges::iterator_t<V>, ranges::iterator_t<Base>>;  // (2) (since C++23)
private:
constexpr explicit /*iterator*/( ranges::iterator_t<Base> current,
                                 difference_type pos);  // (3) (exposition only*)
```

Construct an iterator.

1) Default constructor. Value-initializes the underlying iterator `current_`
   with `ranges::iterator_t<Base>()` and the underlying index `pos_` with `​0​`.

2) Conversion from `/*iterator*/<false>` to `/*iterator*/<true>`. Initializes
   `current_` with `std::move(i.current_)` and `pos_` with `i.pos_`.

3) A private constructor which is used by `enumerate_view::begin` and
   `enumerate_view::end`. This constructor is not accessible to users.
   Initializes `current_` with `std::move(current)` and `pos_` with `pos`.

### Parameters

- **i** — an `/*iterator*/<false>`

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/enumerate_view/iterator/iterator*
