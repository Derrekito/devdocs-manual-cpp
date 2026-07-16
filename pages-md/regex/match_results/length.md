# std::match_results<BidirIt,Alloc>::length

```cpp
difference_type length( size_type n = 0 ) const;  // (since C++11)
```

Returns the length of the specified sub-match.

If `n == 0`, the length of the entire matched expression is returned.

If `n > 0 && n < size()`, the length of `n`th sub-match is returned.

if `n >= size()`, a length of the unmatched match is returned.

The call is equivalent to `(*this)[n].length()`.

### Parameters

- **n** — integral number specifying which match to examine

### Return value

The length of the specified match or sub-match.

### See also

- **operator[]** — returns specified sub-match (public member function)
- **length** — returns the length of the match (if any) (public member function
  of `std::sub_match<BidirIt>`)

---
*Source: https://en.cppreference.com/w/cpp/regex/match_results/length*
