# std::coroutine_handle<Promise>::done

```cpp
Member of other specializations
bool done() const;  // (1) (since C++20)
Member of specialization std::coroutine_handle<std::noop_coroutine_promise>
constexpr bool done() const noexcept;  // (2) (since C++20)
```

Checks if a suspended coroutine is suspended at its final suspended point.

1) Returns `true` if the coroutine to which `*this` refers is suspended at its
   final suspend point, or `false` if the coroutine is suspended at other
   suspend points. The behavior is undefined if `*this` does not refer to a
   suspended coroutine.

2) Always returns `false`.

### Parameters

(none)

### Return value

1) `true` if the coroutine is suspended at its final suspend point, `false` if
   the coroutine is suspended at other suspend points.

2) `false`

### Notes

A no-op coroutine is always considered not suspended at its final suspended
point.

### Example

---
*Source: https://en.cppreference.com/w/cpp/coroutine/coroutine_handle/done*
