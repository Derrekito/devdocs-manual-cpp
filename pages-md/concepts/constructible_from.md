# std::constructible_from

```cpp
template< class T, class... Args >
concept constructible_from =
    std::destructible<T> && std::is_constructible_v<T, Args...>;  // (since C++20)
```

The `constructible_from` concept specifies that a variable of type `T` can be
initialized with the given set of argument types `Args...`.

### See also

- **is_constructibleis_trivially_constructibleis_nothrow_constructible
  (C++11)(C++11)(C++11)** — checks if a type has a constructor for specific
  arguments (class template)

---
*Source: https://en.cppreference.com/w/cpp/concepts/constructible_from*
