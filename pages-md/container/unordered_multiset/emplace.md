# std::unordered_multiset<Key,Hash,KeyEqual,Allocator>::emplace

```cpp
template< class... Args >
iterator emplace( Args&&... args );  // (since C++11)
```

Inserts a new element into the container constructed in-place with the given
`args` .

Careful use of `emplace` allows the new element to be constructed while avoiding
unnecessary copy or move operations. The constructor of the new element is
called with exactly the same arguments as supplied to `emplace`, forwarded via
`std::forward<Args>(args)...`.

If after the operation the new number of elements is greater than old
`max_load_factor() * bucket_count()` a rehashing takes place.
If rehashing occurs (due to the insertion), all iterators are invalidated.
Otherwise (no rehashing), iterators are not invalidated.

### Parameters

- **args** — arguments to forward to the constructor of the element

### Return value

Returns an iterator to the inserted element.

### Exceptions

If an exception is thrown for any reason, this function has no effect (strong
exception safety guarantee).

### Complexity

Amortized constant on average, worst case linear in the size of the container.

### Example

### See also

- **emplace_hint** — constructs elements in-place using a hint (public member
  function)
- **insert** — inserts elements or nodes(since C++17) (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_multiset/emplace*
