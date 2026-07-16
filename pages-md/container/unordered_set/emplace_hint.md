# std::unordered_set<Key,Hash,KeyEqual,Allocator>::emplace_hint

```cpp
template< class... Args >
iterator emplace_hint( const_iterator hint, Args&&... args );  // (since C++11)
```

Inserts a new element to the container, using `hint` as a suggestion where the
element should go. The element is constructed in-place, i.e. no copy or move
operations are performed.

The constructor of the element is called with exactly the same arguments as
supplied to the function, forwarded with `std::forward<Args>(args)...`.

If after the operation the new number of elements is greater than old
`max_load_factor() * bucket_count()` a rehashing takes place.
If rehashing occurs (due to the insertion), all iterators are invalidated.
Otherwise (no rehashing), iterators are not invalidated.

### Parameters

- **hint** — iterator, used as a suggestion as to where to insert the new
  element
- **args** — arguments to forward to the constructor of the element

### Return value

Returns an iterator to the newly inserted element.

If the insertion failed because the element already exists, returns an iterator
to the already existing element with the equivalent key.

### Exceptions

If an exception is thrown by any operation, this function has no effect (strong
exception guarantee).

### Complexity

Amortized constant on average, worst case linear in the size of the container.

### Example

### See also

- **emplace** — constructs element in-place (public member function)
- **insert** — inserts elements or nodes(since C++17) (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_set/emplace_hint*
