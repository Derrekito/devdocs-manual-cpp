# std::filesystem::is_regular_file

```cpp
bool is_regular_file( std::filesystem::file_status s ) noexcept;  // (1) (since C++17)
bool is_regular_file( const std::filesystem::path& p );  // (2) (since C++17)
bool is_regular_file( const std::filesystem::path& p, std::error_code& ec ) noexcept;  // (3) (since C++17)
```

Checks if the given file status or path corresponds to a regular file.

1) Equivalent to `s.type() == file_type::regular`.

2,3) Equivalent to `is_regular_file(status(p))` or `is_regular_file(status(p,
   ec))` respectively.

### Parameters

- **s** — file status to check
- **p** — path to examine
- **ec** — error code to store the error status to

### Return value

`true` if the file indicated by `p` or if the type indicated by `s` refers to a
regular file, `false` otherwise. The non-throwing overload returns `false` if an
error occurs.

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

2,3) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Notes

The throwing overload is additionally specified to throw
`std::filesystem::filesystem_error` if `status(p)` would throw.

### Example

```cpp
#include <cstdio>
#include <cstring>
#include <filesystem>
#include <fstream>
#include <iostream>
#include <sys/socket.h>
#include <sys/stat.h>
#include <sys/un.h>
#include <unistd.h>

namespace fs = std::filesystem;

void demo_status(const fs::path& p, fs::file_status s)
{
    std::cout << p;
    // alternative: switch(s.type()) { case fs::file_type::regular: ...}
    if (fs::is_regular_file(s))
        std::cout << " is a regular file\n";
    if (fs::is_directory(s))
        std::cout << " is a directory\n";
    if (fs::is_block_file(s))
        std::cout << " is a block device\n";
    if (fs::is_character_file(s))
        std::cout << " is a character device\n";
    if (fs::is_fifo(s))
        std::cout << " is a named IPC pipe\n";
    if (fs::is_socket(s))
        std::cout << " is a named IPC socket\n";
    if (fs::is_symlink(s))
        std::cout << " is a symlink\n";
    if (!fs::exists(s))
        std::cout << " does not exist\n";
}

int main()
{
    // create files of different kinds
    fs::create_directory("sandbox");
    fs::create_directory("sandbox/dir");
    std::ofstream{"sandbox/file"}; // create regular file
    fs::create_symlink("file", "sandbox/symlink");

    mkfifo("sandbox/pipe", 0644);
    sockaddr_un addr;
    addr.sun_family = AF_UNIX;
    std::strcpy(addr.sun_path, "sandbox/sock");
    int fd = socket(PF_UNIX, SOCK_STREAM, 0);
    bind(fd, reinterpret_cast<sockaddr*>(&addr), sizeof addr);

    // demo different status accessors
    for (auto it{fs::directory_iterator("sandbox")}; it != fs::directory_iterator(); ++it)
        demo_status(*it, it->symlink_status()); // use cached status from directory entry
    demo_status("/dev/null", fs::status("/dev/null")); // direct calls to status
    demo_status("/dev/sda", fs::status("/dev/sda"));
    demo_status("sandbox/no", fs::status("/sandbox/no"));

    // cleanup (prefer std::unique_ptr-based custom deleters)
    close(fd);
    fs::remove_all("sandbox");
}
```

Possible output:

```text
"sandbox/file" is a regular file
"sandbox/dir" is a directory
"sandbox/pipe" is a named IPC pipe
"sandbox/sock" is a named IPC socket
"sandbox/symlink" is a symlink
"/dev/null" is a character device
"/dev/sda" is a block device
"sandbox/no" does not exist
```

### See also

- **file_type (C++17)** — the type of a file (enum)
- **statussymlink_status (C++17)(C++17)** — determines file attributes
  determines file attributes, checking the symlink target (function)
- **file_status (C++17)** — represents file type and permissions (class)
- **status_known (C++17)** — checks whether file status is known (function)
- **is_block_file (C++17)** — checks whether the given path refers to block
  device (function)
- **is_character_file (C++17)** — checks whether the given path refers to a
  character device (function)
- **is_directory (C++17)** — checks whether the given path refers to a directory
  (function)
- **is_fifo (C++17)** — checks whether the given path refers to a named pipe
  (function)
- **is_other (C++17)** — checks whether the argument refers to an *other* file
  (function)
- **is_socket (C++17)** — checks whether the argument refers to a named IPC
  socket (function)
- **is_symlink (C++17)** — checks whether the argument refers to a symbolic link
  (function)
- **exists (C++17)** — checks whether path refers to existing file system object
  (function)
- **is_regular_file** — checks whether the directory entry refers to a regular
  file (public member function of `std::filesystem::directory_entry`)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/is_regular_file*
