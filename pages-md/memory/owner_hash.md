# std::owner_hash

```cpp
struct owner_hash;  // (since C++26)
```

This function object provides owner-based (as opposed to value-based) hashing of
both `std::weak_ptr` and `std::shared_ptr`.

### Nested types

- **`is_transparent`** — unspecified

### Member functions

- ****operator()**** — calculates the hash of the shared-ownership pointer
  (function)

## std::owner_less::operator()

```cpp
template< class T >
std::size_t operator()( const std::shared_ptr<T>& key ) const noexcept;  // (1) (since C++26)
template< class T >
std::size_t operator()( const std::weak_ptr<T>& key ) const noexcept;  // (2) (since C++26)
```

Equivalent to `return key.owner_hash();`.

### Parameters

- **key** — shared-ownership pointer to be hashed

### Return value

A hash value that is identical for any `std::shared_ptr` or `std::weak_ptr`
object sharing the same ownership.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_smart_ptr_owner_equality` | 202306L | (C++26) | Enabling the use of
      `std::shared_ptr` and `std::weak_ptr` as keys in unordered associative
      containers

### See also

- **owner_hash (C++26)** — provides owner-based hashing of shared pointers
  (public member function of `std::shared_ptr<T>`)
- **owner_hash (C++26)** — provides owner-based hashing of weak pointers (public
  member function of `std::weak_ptr<T>`)

---
*Source: https://en.cppreference.com/w/cpp/memory/owner_hash*
