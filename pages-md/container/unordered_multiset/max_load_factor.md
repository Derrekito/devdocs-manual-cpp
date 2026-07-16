# std::unordered_multiset<Key,Hash,KeyEqual,Allocator>::max_load_factor

```cpp
float max_load_factor() const;  // (1) (since C++11)
void max_load_factor( float ml );  // (2) (since C++11)
```

Manages the maximum load factor (number of elements per bucket). The container
automatically increases the number of buckets if the load factor exceeds this
threshold.

1) Returns current maximum load factor.

2) Sets the maximum load factor to `ml`.

### Parameters

- **ml** — new maximum load factor setting

### Return value

1) Current maximum load factor.

2) (none)

### Complexity

Constant.

### See also

- **load_factor** — returns average number of elements per bucket (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_multiset/max_load_factor*
