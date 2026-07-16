# std::filesystem::path::empty

```cpp
bool empty() const noexcept;  // (since C++17) (until C++20)
[[nodiscard]] bool empty() const noexcept;  // (since C++20)
```

Checks if the path in generic format is empty.

### Parameters

(none)

### Return value

`true` if the path is empty, `false` otherwise.

### Notes

An empty path can be obtained by calling `clear` and by default-constructing a
`path`. It can also be returned by a path decomposition function (such as
`extension`) if the corresponding component is not present in the path.

An empty path is classified as a relative path.

### See also

- **(constructor)** — constructs a `path` (public member function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/empty*
