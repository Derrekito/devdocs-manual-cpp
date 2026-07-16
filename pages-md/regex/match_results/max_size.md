# std::match_results<BidirIt,Alloc>::max_size

```cpp
size_type max_size() const noexcept;  // (since C++11)
```

Returns the maximum number of submatches the `match_results` type is able to
hold due to system or library implementation limitations, i.e.
`std::distance(begin(), end())` for the largest number of submatches.

### Parameters

(none)

### Return value

Maximum number of submatches.

### Complexity

Constant.

---
*Source: https://en.cppreference.com/w/cpp/regex/match_results/max_size*
