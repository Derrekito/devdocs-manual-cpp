# std::generator<Ref,V,Allocator>::generator

```cpp
generator( const generator& ) = delete;  // (1) (since C++23)
generator( generator&& other ) noexcept;  // (2) (since C++23)
```

Constructs a `generator`.

1) The copy-constructor is deleted.

2) The move-constructor that initializes the underlying `coroutine_` with
   `std::exchange(other.coroutine_, {})`, and the underlying stack of coroutine
   handles (`active_`) with `std::exchange(other.active_, nullptr)`. Note, that
   the iterators, previously obtained from `other`, are not invalidated, but
   become iterators into `*this`.

### Parameters

- **other** — a generator object to be moved in

### Example

---
*Source: https://en.cppreference.com/w/cpp/coroutine/generator/generator*
