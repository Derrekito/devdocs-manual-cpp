# std::filesystem::file_type

```cpp
enum class file_type {
    none = /* unspecified */,
    not_found = /* unspecified */,
    regular = /* unspecified */,
    directory = /* unspecified */,
    symlink = /* unspecified */,
    block = /* unspecified */,
    character = /* unspecified */,
    fifo = /* unspecified */,
    socket = /* unspecified */,
    unknown = /* unspecified */,
    /* implementation-defined */
};  // (since C++17)
```

`file_type` defines constants that indicate a type of a file or directory a path
refers to. The value of the enumerators are distinct.

### Constants

- **`none`** — indicates that the file status has not been evaluated yet, or an
  error occurred when evaluating it
- **`not_found`** — indicates that the file was not found (this is not
  considered an error)
- **`regular`** — a regular file
- **`directory`** — a directory
- **`symlink`** — a symbolic link
- **`block`** — a block special file
- **`character`** — a character special file
- **`fifo`** — a FIFO (also known as pipe) file
- **`socket`** — a socket file
- **implementation-defined** — an additional implementation-defined constant for
  each additional file type supported by the implementation (e.g. MSVC STL
  defines `junction` for NTFS junctions)
- **`unknown`** — the file exists but its type could not be determined

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
    switch (s.type())
    {
        case fs::file_type::none:
            std::cout << " has `not-evaluated-yet` type";
            break;
        case fs::file_type::not_found:
            std::cout << " does not exist";
            break;
        case fs::file_type::regular:
            std::cout << " is a regular file";
            break;
        case fs::file_type::directory:
            std::cout << " is a directory";
            break;
        case fs::file_type::symlink:
            std::cout << " is a symlink";
            break;
        case fs::file_type::block:
            std::cout << " is a block device";
            break;
        case fs::file_type::character:
            std::cout << " is a character device";
            break;
        case fs::file_type::fifo:
            std::cout << " is a named IPC pipe";
            break;
        case fs::file_type::socket:
            std::cout << " is a named IPC socket";
            break;
        case fs::file_type::unknown:
            std::cout << " has `unknown` type";
            break;
        default:
            std::cout << " has `implementation-defined` type";
            break;
    }
    std::cout << '\n';
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

- **file_status (C++17)** — represents file type and permissions (class)
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
- **is_regular_file** — checks whether the directory entry refers to a regular
  file (public member function of `std::filesystem::directory_entry`)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/file_type*
