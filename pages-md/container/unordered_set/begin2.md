# std::unordered_set<Key,Hash,KeyEqual,Allocator>::begin(size_type), std::unordered_set<Key,Hash,KeyEqual,Allocator>::cbegin(size_type)

```cpp
local_iterator begin( size_type n );  // (since C++11)
const_local_iterator begin( size_type n ) const;  // (since C++11)
const_local_iterator cbegin( size_type n ) const;  // (since C++11)
```

Returns an iterator to the first element of the bucket with index `n`.

### Parameters

- **n** — the index of the bucket to access

### Return value

Iterator to the first element.

### Complexity

Constant.

### See also

- **end(size_type)cend(size_type)** — returns an iterator to the end of the
  specified bucket (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_set/begin2*
