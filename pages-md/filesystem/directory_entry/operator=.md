# std::filesystem::directory_entry::operator=

```cpp
directory_entry& operator=( const directory_entry& other ) = default;  // (1) (since C++17)
directory_entry& operator=( directory_entry&& other ) noexcept = default;  // (2) (since C++17)
```

Replaces the contents of the directory entry (path and cached attributes, if
any) with the contents of `other`.

Both copy- and move-assignment operators for `directory_entry` are defaulted.

### Parameters

- **other** — other `directory_entry`

### Return value

`*this`

### Example

### See also

- **assign** — assigns contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/directory_entry/operator%3D*
