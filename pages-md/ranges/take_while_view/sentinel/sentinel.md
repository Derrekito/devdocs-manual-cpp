# std::ranges::take_while_view<V,Pred>::*sentinel*<Const>::*sentinel*

```cpp
/*sentinel*/() = default;  // (1) (since C++20)
constexpr explicit /*sentinel*/( ranges::sentinel_t<Base> end, const Pred* pred );  // (2) (since C++20)
constexpr /*sentinel*/( /*sentinel*/<!Const> s )
    requires Const &&
             std::convertible_to<ranges::sentinel_t<V>, ranges::sentinel_t<Base>>;  // (3) (since C++20)
```

Constructs a sentinel.

1) Default constructor. Value-initializes the underlying sentinel and the
   pointer to predicate.

2) Initializes the underlying sentinel with `end` and the pointer to predicate
   with `pred`.

3) Conversion from `/*sentinel*/<false>` to `/*sentinel*/<true>`. Copy
   constructs corresponding members.

### Parameters

- **end** — a sentinel representing the end of (possibly const-qualified) `V`
- **pred** — a pointer to predicate
- **i** — a `/*sentinel*/<false>`

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/take_while_view/sentinel/sentinel*
