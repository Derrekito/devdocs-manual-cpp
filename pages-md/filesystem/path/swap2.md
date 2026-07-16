# std::filesystem::swap(std::filesystem::path)

```cpp
void swap( std::filesystem::path& lhs, std::filesystem::path& rhs ) noexcept;  // (since C++17)
```

Exchanges the state of `lhs` with that of `rhs`. Effectively calls
`lhs.swap(rhs)`.

### Parameters

- **lhs, rhs** — paths whose states to swap

### Return value

(none)

### See also

- **swap** — swaps two paths (public member function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/swap2*
