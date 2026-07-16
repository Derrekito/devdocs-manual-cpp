# std::unordered_set<Key,Hash,KeyEqual,Allocator>::emplace

```cpp
template< class... Args >
std::pair<iterator, bool> emplace( Args&&... args );  // (since C++11)
```

Inserts a new element into the container constructed in-place with the given
`args` if there is no element with the key in the container.

Careful use of `emplace` allows the new element to be constructed while avoiding
unnecessary copy or move operations. The constructor of the new element is
called with exactly the same arguments as supplied to `emplace`, forwarded via
`std::forward<Args>(args)...`. The element may be constructed even if there
already is an element with the key in the container, in which case the newly
constructed element will be destroyed immediately.

If after the operation the new number of elements is greater than old
`max_load_factor() * bucket_count()` a rehashing takes place.
If rehashing occurs (due to the insertion), all iterators are invalidated.
Otherwise (no rehashing), iterators are not invalidated.

### Parameters

- **args** — arguments to forward to the constructor of the element

### Return value

Returns a pair consisting of an iterator to the inserted element, or the
already-existing element if no insertion happened, and a bool denoting whether
the insertion took place (true if insertion happened, false if it did not).

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
*Source: https://en.cppreference.com/w/cpp/container/unordered_set/emplace*
