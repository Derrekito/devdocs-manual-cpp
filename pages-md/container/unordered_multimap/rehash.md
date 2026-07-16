# std::unordered_multimap<Key,T,Hash,KeyEqual,Allocator>::rehash

```cpp
void rehash( size_type count );  // (since C++11)
```

Changes the number of buckets to a value `n` that is not less than `count` and
satisfies `n >= size() / max_load_factor()`, then rehashes the container, i.e.
puts the elements into appropriate buckets considering that total number of
buckets has changed.

### Parameters

- **count** — lower bound for the new number of buckets

### Return value

(none)

### Complexity

Average case linear in the size of the container, worst case quadratic.

### Notes

`rehash(0)` may be used to force an unconditional rehash, such as after
suspension of automatic rehashing by temporarily increasing `max_load_factor()`.

### See also

- **reserve** — reserves space for at least the specified number of elements and
  regenerates the hash table (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_multimap/rehash*
