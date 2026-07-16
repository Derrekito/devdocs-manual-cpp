# std::ranges::chunk_by_view<V,Pred>::begin

```cpp
constexpr /*iterator*/ begin();  // (since C++23)
```

Returns an iterator to the first element of the `chunk_by_view`.

Equivalent to:

```cpp
ranges::iterator_t<V> iter;

if (begin_.has_value())
    iter = begin_.value();
else
{
    iter = /*find_next*/(ranges::begin(base()));
    begin_ = iter; // caching
}

return /*iterator*/(*this, ranges::begin(base()), iter);
```

The behavior is undefined if the underlying predicate `pred_` does not contain a
value.

### Parameters

(none)

### Return value

Iterator to the first element.

### Notes

In order to provide the amortized constant-time complexity required by the
`range` concept, this function caches the result within the data member `begin_`
(the name is for exposition only) for use on subsequent calls.

### Example

### See also

- **end (C++23)** — returns an iterator or a sentinel to the end (public member
  function)
- **ranges::begin (C++20)** — returns an iterator to the beginning of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_by_view/begin*
