# std::unordered_map<Key,T,Hash,KeyEqual,Allocator>::end(size_type), std::unordered_map<Key,T,Hash,KeyEqual,Allocator>::cend(size_type)

```cpp
local_iterator end( size_type n );  // (since C++11)
const_local_iterator end( size_type n ) const;  // (since C++11)
const_local_iterator cend( size_type n ) const;  // (since C++11)
```

Returns an iterator to the element following the last element of the bucket with
index `n`. This element acts as a placeholder, attempting to access it results
in undefined behavior.

### Parameters

- **n** — the index of the bucket to access

### Return value

Iterator to the element following the last element.

### Complexity

Constant.

### See also

- **begin(size_type)cbegin(size_type)** — returns an iterator to the beginning
  of the specified bucket (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_map/end2*
