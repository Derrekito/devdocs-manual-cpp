# std::ranges::take_while_view<V,Pred>::pred

```cpp
constexpr const Pred& pred() const;  // (since C++20)
```

Returns a reference to the stored predicate.

If `*this` does not store a predicate (e.g. an exception is thrown on the
assignment to `*this`, which copy-constructs or move-constructs a `Pred`), the
behavior is undefined.

### Parameters

(none)

### Return value

A reference to the stored predicate.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/take_while_view/pred*
