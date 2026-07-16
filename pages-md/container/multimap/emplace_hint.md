# std::multimap<Key,T,Compare,Allocator>::emplace_hint

```cpp
template< class... Args >
iterator emplace_hint( const_iterator hint, Args&&... args );  // (since C++11)
```

Inserts a new element into the container as close as possible to the position
just before `hint`. The element is constructed in-place, i.e. no copy or move
operations are performed.

The constructor of the element type (`value_type`, that is, `std::pair<const
Key, T>`) is called with exactly the same arguments as supplied to the function,
forwarded with `std::forward<Args>(args)...`.

No iterators or references are invalidated.

### Parameters

- **hint** — iterator to the position before which the new element will be
  inserted
- **args** — arguments to forward to the constructor of the element

### Return value

Returns an iterator to the newly inserted element.

### Exceptions

If an exception is thrown by any operation, this function has no effect (strong
exception guarantee).

### Complexity

Logarithmic in the size of the container in general, but amortized constant if
the new element is inserted just before `hint`.

### See also

- **emplace (C++11)** — constructs element in-place (public member function)
- **insert** — inserts elements or nodes(since C++17) (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/multimap/emplace_hint*
