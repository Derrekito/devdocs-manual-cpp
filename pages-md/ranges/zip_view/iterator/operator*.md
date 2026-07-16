# std::ranges::zip_view<Views...>::*iterator*<Const>::operator*

```cpp
constexpr auto operator*() const;  // (since C++23)
```

Returns a `std::tuple` that consists of underlying pointed-to elements.

Let `current_` denote the underlying tuple-like object that holds iterators to
elements of adapted views. Equivalent to:

```cpp
return /*tuple-transform*/([](auto& i) -> decltype(auto) { return *i; }, current_);
```

### Parameters

(none)

### Return value

The current tuple-like element.

### Notes

`operator->` is not provided.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_view/iterator/operator**
