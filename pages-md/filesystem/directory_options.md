# std::filesystem::directory_options

```cpp
enum class directory_options {
    none = /* unspecified */,
    follow_directory_symlink = /* unspecified */,
    skip_permission_denied = /* unspecified */
};  // (since C++17)
```

This type represents available options that control the behavior of the
`directory_iterator` and `recursive_directory_iterator`.

`directory_options` satisfies the requirements of BitmaskType (which means the
bitwise operators `operator&`, `operator|`, `operator^`, `operator~`,
`operator&=`, `operator|=`, and `operator^=` are defined for this type). `none`
represents the empty bitmask; every other enumerator represents a distinct
bitmask element.

### Member constants

  Member constant | Meaning
  `none` | (Default) Skip directory symlinks, permission denied is error.
  `follow_directory_symlink` | Follow rather than skip directory symlinks.
  `skip_permission_denied` | Skip directories that would otherwise result in
      permission denied errors.

### Example

### See also

- **(constructor)** — constructs a directory iterator (public member function of
  `std::filesystem::directory_iterator`)
- **(constructor)** — constructs a recursive directory iterator (public member
  function of `std::filesystem::recursive_directory_iterator`)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/directory_options*
