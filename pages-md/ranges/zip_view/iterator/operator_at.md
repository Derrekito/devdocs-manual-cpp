# std::ranges::zip_view<Views...>::*iterator*<Const>::operator[]

```cpp
constexpr auto operator[]( difference_type n ) const
    requires /*all-random-access*/<Const, Views...>;  // (since C++23)
```

Obtains a `std::tuple` that consists of underlying pointed-to elements at given
offset relative to current location.

Equivalent to:

```cpp
return /*tuple-transform*/([&]<class I>(I& i) -> decltype(auto) {
           return i[iter_difference_t<I>(n)];
       }, current_);
```

### Parameters

- **n** — position relative to current location

### Return value

The obtained tuple-like element.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_view/iterator/operator_at*
