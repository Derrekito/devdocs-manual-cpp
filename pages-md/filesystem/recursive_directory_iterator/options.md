# std::filesystem::recursive_directory_iterator::options

```cpp
std::filesystem::directory_options options() const;  // (since C++17)
```

Returns the options that affect the directory iteration. The options can only be
supplied when constructing the directory iterator.

If the options argument was not supplied, returns
`std::filesystem::directory_options::none`.

### Parameters

(none)

### Return value

The effective options that affect the directory iteration.

### Exceptions

Throws nothing.

---
*Source: https://en.cppreference.com/w/cpp/filesystem/recursive_directory_iterator/options*
