# std::filesystem::permissions

```cpp
void permissions( const std::filesystem::path& p,
                  std::filesystem::perms prms,
                  std::filesystem::perm_options opts = perm_options::replace );  // (1) (since C++17)
void permissions( const std::filesystem::path& p,
                  std::filesystem::perms prms,
                  std::error_code& ec ) noexcept;  // (2) (since C++17)
void permissions( const std::filesystem::path& p,
                  std::filesystem::perms prms,
                  std::filesystem::perm_options opts,
                  std::error_code& ec );  // (3) (since C++17)
```

Changes access permissions of the file to which `p` resolves, as if by POSIX
`fchmodat`. Symlinks are followed unless `perm_options::nofollow` is set in
`opts`.

The second signature behaves as if called with `opts` set to
`perm_options::replace`.

The effects depend on `prms` and `opts` as follows:

- If `opts` is `perm_options::replace`, file permissions are set to exactly
  `prms & std::filesystem::perms::mask` (meaning, every valid bit of `prms` is
  applied).
- If `opts` is `perm_options::add`, the file permissions are set to exactly
  `status(p).permissions() | (prms & perms::mask)` (meaning, any valid bit that
  is set in `prms`, but not in the file's current permissions is added to the
  file's permissions).
- If `opts` is `perm_options::remove`, the file permissions are set to exactly
  `status(p).permissions() & ~(prms & perms::mask)` (meaning, any valid bit that
  is clear in `prms`, but set in the file's current permissions is cleared in
  the file's permissions).

`opts` is required to have only one of `replace`, `add`, or `remove` to be set.

The non-throwing overload has no special action on error.

### Parameters

- **p** — path to examine
- **prms** — permissions to set, add, or remove
- **opts** — options controlling the action taken by this function
- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

(none)

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

1) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `p` as the first path argument and the OS error code as the
   error code argument.

2,3) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Notes

Permissions may not necessarily be implemented as bits, but they are treated
that way conceptually.

Some permission bits may be ignored on some systems, and changing some bits may
automatically change others (e.g. on platforms without owner/group/all
distinction, setting any of the three write bits set all three).

### Example

```cpp
#include <filesystem>
#include <fstream>
#include <iostream>

void demo_perms(std::filesystem::perms p)
{
    using std::filesystem::perms;
    auto show = [=](char op, perms perm)
    {
        std::cout << (perms::none == (perm & p) ? '-' : op);
    };
    show('r', perms::owner_read);
    show('w', perms::owner_write);
    show('x', perms::owner_exec);
    show('r', perms::group_read);
    show('w', perms::group_write);
    show('x', perms::group_exec);
    show('r', perms::others_read);
    show('w', perms::others_write);
    show('x', perms::others_exec);
    std::cout << '\n';
}

int main()
{
    std::ofstream("test.txt"); // create file

    std::cout << "Created file with permissions: ";
    demo_perms(std::filesystem::status("test.txt").permissions());

    std::filesystem::permissions(
        "test.txt",
        std::filesystem::perms::owner_all | std::filesystem::perms::group_all,
        std::filesystem::perm_options::add
    );

    std::cout << "After adding u+rwx and g+rwx:  ";
    demo_perms(std::filesystem::status("test.txt").permissions());

    std::filesystem::remove("test.txt");
}
```

Possible output:

```text
Created file with permissions: rw-r--r--
After adding u+rwx and g+wrx:  rwxrwxr--
```

### See also

- **perms (C++17)** — identifies file system permissions (enum)
- **statussymlink_status (C++17)(C++17)** — determines file attributes
  determines file attributes, checking the symlink target (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/permissions*
