# std::match_results<BidirIt,Alloc>::empty

```cpp
bool empty() const;  // (since C++11) (until C++20)
[[nodiscard]] bool empty() const;  // (since C++20)
```

Checks whether the match was successful.

### Parameters

(none)

### Return value

`true` if `*this` represents the result of a failed match, `false` otherwise,
i.e. `size() == 0`.

### Exceptions

May throw implementation-defined exceptions.

### Complexity

Constant.

### See also

- **size** — returns the number of matches in a fully-established result state
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/regex/match_results/empty*
