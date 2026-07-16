# std::sub_match<BidirIt>::length

```cpp
difference_type length() const;
```

### Parameters

(none)

### Return value

Returns the number of characters in the match, i.e. `std::distance(first,
second)` if the match is valid, 0 otherwise.

### Complexity

Constant.

---
*Source: https://en.cppreference.com/w/cpp/regex/sub_match/length*
