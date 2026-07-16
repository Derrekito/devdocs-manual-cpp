# std::filesystem::recursive_directory_iterator::recursion_pending

```cpp
bool recursion_pending() const;  // (since C++17)
```

Returns `true` if the next increment will cause the directory currently referred
to by `*this` to be iterated into.

This function returns `true` immediately after construction or an increment.
Recursion can be disabled via `disable_recursion_pending()`.

### Parameters

(none)

### Return value

`true` if the next increment will iterate into the currently referred directory,
`false` otherwise.

### Exceptions

Throws nothing.

### Example

---
*Source: https://en.cppreference.com/w/cpp/filesystem/recursive_directory_iterator/recursion_pending*
