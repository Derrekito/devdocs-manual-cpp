# std::filesystem::perm_options

```cpp
enum class perm_options {
    replace = /* unspecified */,
    add = /* unspecified */,
    remove = /* unspecified */,
    nofollow = /* unspecified */
};  // (since C++17)
```

This type represents available options that control the behavior of the function
`std::filesystem::permissions()`.

`perm_options` satisfies the requirements of BitmaskType (which means the
bitwise operators `operator&`, `operator|`, `operator^`, `operator~`,
`operator&=`, `operator|=`, and `operator^=` are defined for this type).

### Member constants

At most one of `add`, `remove`, `replace` may be present, otherwise the behavior
of the permissions function is undefined.

  Member constant | Meaning
  `replace` | Permissions will be completely replaced by the argument to
      `permissions()` (default behavior)
  `add` | Permissions will be replaced by the bitwise OR of the argument and the
      current permissions
  `remove` | Permissions will be replaced by the bitwise AND of the negated
      argument and current permissions
  `nofollow` | Permissions will be changed on the symlink itself, rather than on
      the file it resolves to

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

- **permissions (C++17)** — modifies file access permissions (function)
- **perms (C++17)** — identifies file system permissions (enum)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/perm_options*
