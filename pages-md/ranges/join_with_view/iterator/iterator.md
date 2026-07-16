# std::ranges::join_with_view<V,Pattern>::*iterator*<Const>::*iterator*

```cpp
/*iterator*/() requires std::default_initializable<ranges::iterator_t<Base>> = default;  // (1) (since C++23)
constexpr /*iterator*/( /*iterator*/<!Const> i )
  requires Const &&
    std::convertible_to<ranges::iterator_t<V>, ranges::iterator_t<Base>> &&
    std::convertible_to<ranges::iterator_t<ranges::range_reference_t<V>>,
                        ranges::iterator_t<InnerBase>> &&
    std::convertible_to<ranges::iterator_t<Pattern>, ranges::iterator_t<PatternBase>>;  // (2) (since C++23)
```

Construct an iterator.

1) Default constructor. Value-initializes the underlying iterator to `Base` and
   the iterator to `PatternBase`, and initializes the pointer to parent
   `join_with_view` with `nullptr`.

2) Conversion from `/*iterator*/<false>` to `/*iterator*/<true>`. Move
   constructs corresponding members.

This iterator also has a private constructor which is used by
`join_with_view::begin` and `join_with_view::end`. This constructor is not
accessible to users.

### Parameters

- **i** — an `/*iterator*/<false>`

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_with_view/iterator/iterator*
