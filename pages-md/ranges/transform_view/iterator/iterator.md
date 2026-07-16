# std::ranges::transform_view<V,F>::*iterator*<Const>::*iterator*

```cpp
/*iterator*/() requires std::default_initializable<ranges::iterator_t<Base>> = default;  // (1) (since C++20)
constexpr /*iterator*/( Parent& parent, ranges::iterator_t<Base> current );  // (2) (since C++20)
constexpr /*iterator*/( /*iterator*/<!Const> i )
  requires Const &&
    std::convertible_to<ranges::iterator_t<V>, ranges::iterator_t<Base>>;  // (3) (since C++20)
```

Construct an iterator.

1) Default constructor. Value-initializes the underlying iterator, and
   initializes the pointer to parent `transform_view` with `nullptr`.

2) Initializes the underlying iterator with `std::move(current)`, and the
   pointer to parent with `std::addressof(parent)`.

3) Conversion from `/*iterator*/<false>` to `/*iterator*/<true>`. Move
   constructs corresponding members.

### Parameters

- **parent** — a (possibly const-qualified) `std::ranges::transform_view`
- **current** — an iterator into (possibly const-qualified) `V`
- **i** — an `/*iterator*/<false>`

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/transform_view/iterator/iterator*
