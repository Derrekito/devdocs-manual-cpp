# std::unordered_multiset<Key,Hash,KeyEqual,Allocator>::reserve

```cpp
void reserve( size_type count );  // (since C++11)
```

Sets the number of buckets to the number needed to accommodate at least `count`
elements without exceeding maximum load factor and rehashes the container, i.e.
puts the elements into appropriate buckets considering that total number of
buckets has changed. Effectively calls `rehash(std::ceil(count /
max_load_factor()))`.

### Parameters

- **count** — new capacity of the container

### Return value

(none)

### Complexity

Average case linear in the size of the container, worst case quadratic.

### See also

- **rehash** — reserves at least the specified number of buckets and regenerates
  the hash table (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_multiset/reserve*
