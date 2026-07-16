# std::ranges::elements_view<V,F>::*iterator*<Const>::*iterator*

```cpp
/*iterator*/() requires std::default_initializable<ranges::iterator_t<Base>>
    = default;  // (1) (since C++20)
constexpr explicit /*iterator*/( ranges::iterator_t<Base> current );  // (2) (since C++20)
constexpr /*iterator*/( /*iterator*/<!Const> i ) requires Const &&
    std::convertible_to<ranges::iterator_t<V>, ranges::iterator_t<Base>>;  // (3) (since C++20)
```

Construct an iterator.

1) Value-initializes the underlying iterator `current_` via its default member
   initializer (`= ranges::iterator_t<Base>()`).

2) Initializes the underlying iterator `current_` with `std::move(current)`.

3) Conversion from `/*iterator*/<false>` to `/*iterator*/<true>`. Initializes
   the underlying iterator `current_` with `std::move(i.current)`.

### Parameters

- **current** — an iterator into (possibly const-qualified) `V`
- **i** — an `/*iterator*/<false>`

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/elements_view/iterator/iterator*
