# std::filesystem::copy

```cpp
void copy( const std::filesystem::path& from,
           const std::filesystem::path& to );  // (1) (since C++17)
void copy( const std::filesystem::path& from,
           const std::filesystem::path& to,
           std::error_code& ec );  // (2) (since C++17)
void copy( const std::filesystem::path& from,
           const std::filesystem::path& to,
           std::filesystem::copy_options options );  // (3) (since C++17)
void copy( const std::filesystem::path& from,
           const std::filesystem::path& to,
           std::filesystem::copy_options options,
           std::error_code& ec );  // (4) (since C++17)
```

Copies files and directories, with a variety of options.

1,2) The default, equivalent to (3,4) with `copy_options::none` used as
   `options`.

3,4) Copies the file or directory `from` to file or directory `to`, using the
   copy options indicated by `options`. The behavior is undefined if there is
   more than one option in any of the copy_options option group present in
   `options` (even in the `copy_file` group).

The behavior is as follows:

- First, before doing anything else, obtains type and permissions of `from` by
  no more than a single call to
- If necessary, obtains the status of `to`, by no more than a single call to
- If either `from` or `to` has an implementation-defined file type, the effects
  of this function are implementation-defined.
- If `from` does not exist, reports an error.
- If `from` and `to` are the same file as determined by
  `std::filesystem::equivalent`, reports an error.
- If either `from` or `to` is not a regular file, a directory, or a symlink, as
  determined by `std::filesystem::is_other`, reports an error.
- If `from` is a directory, but `to` is a regular file, reports an error.
- If `from` is a symbolic link, then
- Otherwise, if `from` is a regular file, then
- Otherwise, if `from` is a directory and `copy_options::create_symlinks` is set
  in `options`, reports an error with an error code equal to
  `std::make_error_code(std::errc::is_a_directory)`.
- Otherwise, if `from` is a directory and either `options` has
  `copy_options::recursive` or is `copy_options::none`,
- Otherwise does nothing.

### Parameters

- **from** — path to the source file, directory, or symlink
- **to** — path to the target file, directory, or symlink
- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

(none)

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

1,3) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `from` as the first path argument, `to` as the second path
   argument, and the OS error code as the error code argument.

2,4) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Notes

The default behavior when copying directories is the non-recursive copy: the
files are copied, but not the subdirectories:

```cpp
// Given
// /dir1 contains /dir1/file1, /dir1/file2, /dir1/dir2
// and /dir1/dir2 contains /dir1/dir2/file3
// After
std::filesystem::copy("/dir1", "/dir3");
// /dir3 is created (with the attributes of /dir1)
// /dir1/file1 is copied to /dir3/file1
// /dir1/file2 is copied to /dir3/file2
```

While with `copy_options::recursive`, the subdirectories are also copied, with
their content, recursively.

```cpp
// ...but after
std::filesystem::copy("/dir1", "/dir3", std::filesystem::copy_options::recursive);
// /dir3 is created (with the attributes of /dir1)
// /dir1/file1 is copied to /dir3/file1
// /dir1/file2 is copied to /dir3/file2
// /dir3/dir2 is created (with the attributes of /dir1/dir2)
// /dir1/dir2/file3 is copied to /dir3/dir2/file3
```

### Example

```cpp
#include <cstdlib>
#include <filesystem>
#include <fstream>
#include <iostream>
namespace fs = std::filesystem;

int main()
{
    fs::create_directories("sandbox/dir/subdir");
    std::ofstream("sandbox/file1.txt").put('a');
    fs::copy("sandbox/file1.txt", "sandbox/file2.txt"); // copy file
    fs::copy("sandbox/dir", "sandbox/dir2"); // copy directory (non-recursive)
    const auto copyOptions = fs::copy_options::update_existing
                           | fs::copy_options::recursive
                           | fs::copy_options::directories_only
                           ;
    fs::copy("sandbox", "sandbox_copy", copyOptions);
    static_cast<void>(std::system("tree"));
    fs::remove_all("sandbox");
    fs::remove_all("sandbox_copy");
}
```

Possible output:

```text
.
├── sandbox
│   ├── dir
│   │   └── subdir
│   ├── dir2
│   ├── file1.txt
│   └── file2.txt
└── sandbox_copy
    ├── dir
    │   └── subdir
    └── dir2

8 directories, 2 files
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3013 | C++17 | `error_code` overload marked noexcept but can allocate
      memory | noexcept removed
  LWG 2682 | C++17 | attempting to create a symlink for a directory succeeds but
      does nothing | reports an error

### See also

- **copy_options (C++17)** — specifies semantics of copy operations (enum)
- **copy_symlink (C++17)** — copies a symbolic link (function)
- **copy_file (C++17)** — copies file contents (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/copy*
