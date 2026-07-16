# std::priority_queue<T,Container,Compare>::operator=

```cpp
priority_queue& operator=( const priority_queue& other );  // (1)
priority_queue& operator=( priority_queue&& other );  // (2) (since C++11)
```

Replaces the contents of the container adaptor with those of `other`.

1) Copy assignment operator. Replaces the contents with a copy of the contents
   of `other`. Effectively calls `c = other.c; comp = other.comp;`. (implicitly
   declared)

2) Move assignment operator. Replaces the contents with those of `other` using
   move semantics. Effectively calls `c = std::move(other.c); comp =
   std::move(other.comp);`. (implicitly declared)

### Parameters

- **other** — another container adaptor to be used as source

### Return value

`*this`

### Complexity

Equivalent to that of `operator=` of the underlying container.

### See also

- **(constructor)** — constructs the `priority_queue` (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/priority_queue/operator%3D*
