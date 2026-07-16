# std::generator<Ref,V,Allocator>::operator=

```cpp
generator& operator=( generator other ) noexcept;  // (since C++23)
```

Replaces the contents of the generator object.

Equivalent to:

```cpp
std::swap(coroutine_, other.coroutine_);
std::swap(active_, other.active_);
```

### Parameters

- **other** — another generator to be copied from

### Return value

`*this`

### Complexity

### Notes

Iterators previously obtained from `other` are not invalidated – they become
iterators into `*this`.

### Example

---
*Source: https://en.cppreference.com/w/cpp/coroutine/generator/operator%3D*
