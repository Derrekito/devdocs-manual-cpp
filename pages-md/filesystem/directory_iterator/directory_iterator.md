# std::filesystem::directory_iterator::directory_iterator

```cpp
directory_iterator() noexcept;  // (1) (since C++17)
explicit directory_iterator( const std::filesystem::path& p );  // (2) (since C++17)
directory_iterator( const std::filesystem::path& p,
                    std::filesystem::directory_options options );  // (3) (since C++17)
directory_iterator( const std::filesystem::path& p, std::error_code& ec );  // (4) (since C++17)
directory_iterator( const std::filesystem::path& p,
                    std::filesystem::directory_options options,
                    std::error_code& ec );  // (5) (since C++17)
directory_iterator( const directory_iterator& other ) = default;  // (6) (since C++17)
directory_iterator( directory_iterator&& other ) = default;  // (7) (since C++17)
```

Constructs a new directory iterator.

1) Constructs the end iterator.

2) Constructs a directory iterator that refers to the first directory entry of a
   directory identified by `p`. If `p` refers to a non-existing file or not a
   directory, throws `std::filesystem::filesystem_error`.

3) Same as (2), but if
   `std::filesystem::directory_options::skip_permission_denied` is set in
   `options` and construction encounters a permissions denied error, constructs
   the end iterator and does not report an error.

4) Constructs a directory iterator that refers to the first directory entry of a
   directory identified by `p`. If `p` refers to a non-existing file or not a
   directory, returns the end iterator and sets `ec`.

5) Same as (4), but if
   `std::filesystem::directory_options::skip_permission_denied` is set in
   `options` and construction encounters a permissions denied error, constructs
   the end iterator and does not report an error.

6) Copy constructor.

7) Move constructor.

### Parameters

- **p** — path to the filesystem object to which the directory iterator will
  refer
- **ec** — out-parameter for error reporting in the non-throwing overloads
- **options** — the set of BitmaskType options that control the behavior of the
  directory iterator
- **other** — another directory iterator to use as source to initialize the
  directory iterator with

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

2,3) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `p` as the first path argument and the OS error code as the
   error code argument.

4,5) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Notes

To iterate over the current directory, construct the iterator as
`directory_iterator(".")` instead of `directory_iterator("")`.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3013 | C++17 | `error_code` overload marked noexcept but can allocate
      memory | noexcept removed

---
*Source: https://en.cppreference.com/w/cpp/filesystem/directory_iterator/directory_iterator*
