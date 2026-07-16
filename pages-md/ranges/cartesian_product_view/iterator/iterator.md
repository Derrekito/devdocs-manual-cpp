# std::ranges::cartesian_product_view<First, Vs...>::*iterator*<Const>::*iterator*

```cpp
/*iterator*/() = default;  // (1) (since C++23)
constexpr /*iterator*/( /*iterator*/<!Const> i )
    requires Const && (
        std::convertible_to<ranges::iterator_t<First>, ranges::iterator_t<const First>>
        && ... &&
        std::convertible_to<ranges::iterator_t<Vs>, ranges::iterator_t<const Vs>>
    );  // (2) (since C++23)
private:
constexpr /*iterator*/(
    /*Parent*/& parent,
    std::tuple<ranges::iterator_t</*maybe-const*/<Const, First>>,
               ranges::iterator_t</*maybe-const*/<Const, Vs>>...> current );  // (3) (exposition only*)
```

Construct an iterator.

1) Default constructor. Value-initializes the `parent_` with `nullptr` and
   default-initializes the `current_`.

2) Conversion from `/*iterator*/<false>` to `/*iterator*/<true>`. Initializes
   `parent_` with `i.parent_` and `current_` with `std::move(i.current_)`.

3) A private constructor which is used by `cartesian_product_view::begin` and
   `cartesian_product_view::end`. This constructor is not accessible to users.
   Initializes `parent_` with `std::addressof(parent)` and `current_` with
   `std::move(current)`.

### Parameters

- **i** — an `/*iterator*/<false>`

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/cartesian_product_view/iterator/iterator*
