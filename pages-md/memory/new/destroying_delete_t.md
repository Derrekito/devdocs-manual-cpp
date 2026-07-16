# std::destroying_delete_t, std::destroying_delete

```cpp
struct destroying_delete_t { explicit destroying_delete_t() = default; };  // (since C++20)
inline constexpr destroying_delete_t destroying_delete{};  // (since C++20)
```

Tag type used to identify the destroying delete form of `operator delete`.

### See also

- **operator deleteoperator delete[]** — deallocation functions (function)

---
*Source: https://en.cppreference.com/w/cpp/memory/new/destroying_delete_t*
