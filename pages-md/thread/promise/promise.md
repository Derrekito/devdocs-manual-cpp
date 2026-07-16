# std::promise<R>::promise

```cpp
promise();  // (1) (since C++11)
template< class Alloc >
promise( std::allocator_arg_t, const Alloc& alloc );  // (2) (since C++11)
promise( promise&& other ) noexcept;  // (3) (since C++11)
promise( const promise& other ) = delete;  // (4) (since C++11)
```

Constructs a `promise` object.

1) Default constructor. Constructs the promise with an empty shared state.

2) Constructs the promise with an empty shared state. The shared state is
   allocated using `alloc`. `Alloc` must meet the requirements of Allocator.

3) Move constructor. Constructs the promise with the shared state of `other`
   using move semantics. After construction, `other` has no shared state.

4) `promise` is not copyable.

### Parameters

- **alloc** — allocator to use to allocate the shared state
- **other** — another `promise` to acquire the state from

### Exceptions

1,2) May throw implementation-defined exceptions.

### Example

---
*Source: https://en.cppreference.com/w/cpp/thread/promise/promise*
