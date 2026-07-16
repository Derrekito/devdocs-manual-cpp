# std::unordered_multiset<Key,Hash,KeyEqual,Allocator>::load_factor

```cpp
float load_factor() const;  // (since C++11)
```

Returns the average number of elements per bucket, that is, `size()` divided by
`bucket_count()`.

### Parameters

(none)

### Return value

Average number of elements per bucket.

### Complexity

Constant.

### See also

- **max_load_factor** — manages maximum average number of elements per bucket
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_multiset/load_factor*
