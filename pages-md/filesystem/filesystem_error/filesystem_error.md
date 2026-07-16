# std::filesystem::filesystem_error::filesystem_error

```cpp
filesystem_error( const std::string& what_arg,
                  std::error_code ec );  // (1) (since C++17)
filesystem_error( const std::string& what_arg,
                  const std::filesystem::path& p1,
                  std::error_code ec );  // (2) (since C++17)
filesystem_error( const std::string& what_arg,
                  const std::filesystem::path& p1,
                  const std::filesystem::path& p2,
                  std::error_code ec );  // (3) (since C++17)
filesystem_error( const filesystem_error& other ) noexcept;  // (4) (since C++17)
```

Constructs a new `filesystem_error` object.

1-3) The error code is set to `ec` and optionally, the paths that were involved
   in the operation that resulted in the error, are set to `p1` and `p2`.
   `what()` after construction returns a string that contains `what_arg`
   (assuming that it does not contain an embedded null character ). If either or
   both `path` arguments are not provided, a null `path` is used instead.

4) Copy constructor. Initialize the contents with those of `other`. If `*this`
   and `other` both have dynamic type `std::filesystem_error::filesystem_error`
   then `std::strcmp(what(), other.what()) == 0`.

### Parameters

- **what_arg** — explanatory string
- **ec** — error code for the specific operating system dependent error
- **p1, p2** — paths involved in the operation raising system error
- **other** — another `filesystem_error` object to copy

### Notes

Because copying `std::filesystem::filesystem_error` is not permitted to throw
exceptions, the explanatory string is typically stored internally in a
separately-allocated reference-counted storage. This is also why there is no
constructor taking `std::string&&`: it would have to copy the content anyway.

Typical implementations also store `path` objects referenced by `path1()` and
`path2()` in the reference-counted storage.

### Example

---
*Source: https://en.cppreference.com/w/cpp/filesystem/filesystem_error/filesystem_error*
