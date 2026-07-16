# std::ranges::join_view<V>::*iterator*<Const>::satisfy

```cpp
constexpr void satisfy();  // (since C++20) (exposition only*)
```

Skips over empty inner ranges and initializes the underlying iterator `inner_`.

Let the constant `/*ref-is-glvalue*/` be
`std::is_reference_v<ranges::range_reference_t<Base>>`.

The function body is equivalent to

```cpp
auto update_inner = [this](const ranges::iterator_t<Base>& x) -> auto&&
{
    if constexpr (/*ref-is-glvalue*/)     // *x is a reference
        return *x;
    else
        return parent_->inner_./*emplace-deref*/(x);
};

for (; outer_ != ranges::end(parent_->base_); ++outer_)
{
    auto&& inner = update_inner(outer_);
    inner_ = ranges::begin(inner);
    if (inner_ != ranges::end(inner))
        return;
}

if constexpr (/*ref-is-glvalue*/)
    inner_ = InnerIter();
```

### Parameters

(none)

### Return value

(none)

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_view/iterator/satisfy*
