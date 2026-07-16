# std::unordered_map<Key,T,Hash,KeyEqual,Allocator>::bucket_size

```cpp
size_type bucket_size( size_type n ) const;  // (since C++11)
```

Returns the number of elements in the bucket with index `n`.

### Parameters

- **n** — the index of the bucket to examine

### Return value

The number of elements in the bucket `n`.

### Complexity

Linear in the size of the bucket `n`.

### See also

- **bucket_count** — returns the number of buckets (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_map/bucket_size*
