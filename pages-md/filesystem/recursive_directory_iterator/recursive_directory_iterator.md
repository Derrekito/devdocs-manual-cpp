# std::filesystem::recursive_directory_iterator::recursive_directory_iterator

```cpp
recursive_directory_iterator() noexcept;  // (1) (since C++17)
recursive_directory_iterator( const recursive_directory_iterator& other );  // (2) (since C++17)
recursive_directory_iterator( recursive_directory_iterator&& other ) noexcept;  // (3) (since C++17)
explicit recursive_directory_iterator( const std::filesystem::path& p );  // (4) (since C++17)
recursive_directory_iterator(
    const std::filesystem::path& p,
    std::filesystem::directory_options options );  // (5) (since C++17)
recursive_directory_iterator(
    const std::filesystem::path& p,
    std::filesystem::directory_options options,
    std::error_code& ec );  // (6) (since C++17)
recursive_directory_iterator( const std::filesystem::path& p, std::error_code& ec );  // (7) (since C++17)
```

Constructs new recursive directory iterator.

1) Default constructor. Constructs an end iterator.

2) Copy constructor.

3) Move constructor.

4-7) Constructs an iterator that refers to the first entry in the directory that
   `p` resolves to.

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

4,5) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `p` as the first path argument and the OS error code as the
   error code argument.

6,7) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Notes

Recursive directory iterators do not follow directory symlinks by default. To
enable this behavior, specify `directory_options::follow_directory_symlink`
among the `options` option set.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3013 | C++17 | `error_code` overload marked noexcept but can allocate
      memory | noexcept removed

---
*Source: https://en.cppreference.com/w/cpp/filesystem/recursive_directory_iterator/recursive_directory_iterator*
