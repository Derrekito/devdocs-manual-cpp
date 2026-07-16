# std::ranges::join_view<V>::*iterator*<Const>::*iterator*

```cpp
/*iterator*/() requires std::default_initializable<OuterIter> &&
                        std::default_initializable<InnerIter> = default;  // (1) (since C++20)
constexpr /*iterator*/( Parent& parent, OuterIter outer );  // (2) (since C++20)
constexpr /*iterator*/( /*iterator*/<!Const> i )
  requires Const &&
           std::convertible_to<ranges::iterator_t<V>, OuterIter> &&
           std::convertible_to<ranges::iterator_t<InnerRng>, InnerIter>;  // (3) (since C++20)
```

Construct an iterator.

1) Default constructor. Value-initializes the underlying iterators, and
   initializes the pointer to parent `ranges::join_view` with `nullptr`.

2) Initializes the underlying `outer_` iterator with `std::move(outer)`, and the
   pointer to parent `parent_` with `std::addressof(parent)`; then calls
   `satisfy()`.

3) Conversion from `/*iterator*/<false>` to `/*iterator*/<true>`. Move
   constructs the underlying iterators `outer_` with `std::move(i.outer_)`,
   `inner_` with `std::move(i.inner_)`, and underlying pointer to parent
   `parent_` with `i.parent_`.

### Parameters

- **parent** — a (possibly const-qualified) `ranges::join_view`
- **outer** — an iterator into (possibly const-qualified)
  `ranges::iterator_t<Base>`
- **i** — an `/*iterator*/<false>`

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_view/iterator/iterator*
