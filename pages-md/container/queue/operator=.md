# std::queue<T,Container>::operator=

```cpp
queue& operator=( const queue& other );  // (1)
queue& operator=( queue&& other );  // (2) (since C++11)
```

Replaces the contents of the container adaptor with those of `other`.

1) Copy assignment operator. Replaces the contents with a copy of the contents
   of `other`. Effectively calls `c = other.c;`. (implicitly declared)

2) Move assignment operator. Replaces the contents with those of `other` using
   move semantics. Effectively calls `c = std::move(other.c);`. (implicitly
   declared)

### Parameters

- **other** — another container adaptor to be used as source

### Return value

`*this`

### Complexity

Equivalent to that of `operator=` of the underlying container.

### See also

- **(constructor)** — constructs the `queue` (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/queue/operator%3D*
