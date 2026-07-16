# std::unordered_map<Key,T,Hash,KeyEqual,Allocator>::bucket

```cpp
size_type bucket( const Key& key ) const;  // (since C++11)
```

Returns the index of the bucket for key `key`. Elements (if any) with keys
equivalent to `key` are always found in this bucket. The returned value is valid
only for instances of the container for which `bucket_count()` returns the same
value.

The behavior is undefined if `bucket_count()` is zero.

### Parameters

- **key** — the value of the key to examine

### Return value

Bucket index for the key `key`.

### Complexity

Constant.

### See also

- **bucket_size** — returns the number of elements in specific bucket (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_map/bucket*
