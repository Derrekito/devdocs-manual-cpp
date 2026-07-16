# std::ranges::chunk_by_view<V,Pred>::pred

```cpp
constexpr const Pred& pred() const;  // (since C++23)
```

Returns a reference to the contained `Pred` object. Equivalent to `return
*pred_;`.

The behavior is undefined if `pred_` does not contain a value.

### Parameters

(none)

### Return value

A copy of the underlying view.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_by_view/pred*
