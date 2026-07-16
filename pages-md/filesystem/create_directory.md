# std::filesystem::create_directory, std::filesystem::create_directories

```cpp
bool create_directory( const std::filesystem::path& p );  // (1) (since C++17)
bool create_directory( const std::filesystem::path& p, std::error_code& ec ) noexcept;  // (2) (since C++17)
bool create_directory( const std::filesystem::path& p,
                       const std::filesystem::path& existing_p );  // (3) (since C++17)
bool create_directory( const std::filesystem::path& p,
                       const std::filesystem::path& existing_p,
                       std::error_code& ec ) noexcept;  // (4) (since C++17)
bool create_directories( const std::filesystem::path& p );  // (5) (since C++17)
bool create_directories( const std::filesystem::path& p, std::error_code& ec );  // (6) (since C++17)
```

1,2) Creates the directory `p` as if by POSIX `mkdir()` with a second argument
   of `static_cast<int>(std::filesystem::perms::all)` (the parent directory must
   already exist). If the function fails because `p` resolves to an existing
   directory, no error is reported. Otherwise on failure an error is reported.

3,4) Same as (1,2), except that the attributes of the new directory are copied
   from `existing_p` (which must be a directory that exists). It is OS-dependent
   which attributes are copied: on POSIX systems, the attributes are copied as
   if by stat(existing_p.c_str(), &attributes_stat) mkdir(p.c_str(),
   attributes_stat.st_mode) On Windows OS, no attributes of `existing_p` are
   copied.

5,6) Executes (1,2) for every element of `p` that does not already exist. If `p`
   already exists, the function does nothing (this condition is not treated as
   an error).

### Parameters

- **p** — the path to the new directory to create
- **existing_p** — the path to a directory to copy the attributes from
- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

`true` if a directory was created for the directory `p` resolves to, `false`
otherwise.

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

1,5) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `p` as the first path argument and the OS error code as the
   error code argument.

2,6) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

3) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `p` as the first path argument, `existing_p` as the second
   path argument, and the OS error code as the error code argument.

4) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Notes

The attribute-preserving overload (3,4) is implicitly invoked by `copy()` when
recursively copying directories. Its equivalent in boost.filesystem is
`copy_directory` (with argument order reversed).

### Example

```cpp
#include <cstdlib>
#include <filesystem>
#include <fstream>
#include <iostream>
namespace fs = std::filesystem;

int main()
{
    fs::current_path(fs::temp_directory_path());
    fs::create_directories("sandbox/1/2/a");
    fs::create_directory("sandbox/1/2/b");
    fs::permissions("sandbox/1/2/b", fs::perms::others_all, fs::perm_options::remove);
    fs::create_directory("sandbox/1/2/c", "sandbox/1/2/b");
    std::system("ls -l sandbox/1/2");
    std::system("tree sandbox");
    fs::remove_all("sandbox");
}
```

Possible output:

```text
drwxr-xr-x 2 user group 4096 Apr 15 09:33 a
drwxr-x--- 2 user group 4096 Apr 15 09:33 b
drwxr-x--- 2 user group 4096 Apr 15 09:33 c
sandbox
└── 1
    └── 2
        ├── a
        ├── b
        └── c
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2935 | C++17 | error if target already exists but is not a directory | not
      error
  LWG 3014 | C++17 | `error_code` overload of `create_directories` marked
      noexcept but can allocate memory | noexcept removed
  P1164R1 | C++17 | creation failure caused by an existing non-directory file is
      not an error | made error

### See also

- **create_symlinkcreate_directory_symlink (C++17)(C++17)** — creates a symbolic
  link (function)
- **copy (C++17)** — copies files or directories (function)
- **perms (C++17)** — identifies file system permissions (enum)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/create_directory*
