# operator==(std::filesystem::file_status)

```cpp
friend bool operator==( const file_status& lhs, const file_status& rhs ) noexcept;  // (since C++20)
```

Checks if two `file_status` values are equal, i.e. types and permissions
represented by them are same respectively.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::filesystem::file_status` is an associated class of the arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **lhs, rhs** — `file_status` values to compare

### Return value

`lhs.type() == rhs.type() && lhs.permissions() == rhs.permissions()`

### See also

- **type** — gets or sets the type of the file (public member function)
- **permissions** — gets or sets the permissions of the file (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/file_status/operator%3D%3D*
