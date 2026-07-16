# std::hash<std::type_index>

```cpp
template<> struct hash<std::type_index>;  // (since C++11)
```

The template specialization of `std::hash` for `std::type_index` allows users to
obtain hashes of objects of type `std::type_index`.

The member `operator()` effectively returns the same value as `hash_code()`.

### See also

- **hash_code** — returns hashed code (public member function)

---
*Source: https://en.cppreference.com/w/cpp/types/type_index/hash*
