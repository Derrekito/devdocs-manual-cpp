# std::filesystem::file_time_type

```cpp
using file_time_type = std::chrono::time_point</*trivial-clock*/>;  // (since C++17) (until C++20)
using file_time_type = std::chrono::time_point<std::chrono::file_clock>;  // (since C++20)
```

Represents file time.

`trivial-clock` is an implementation-defined type that satisfies TrivialClock
and is sufficient to represent the resolution and range of the file time values
offered by the filesystem.
*(until C++20)*

### Example

```cpp
#include <chrono>
#include <filesystem>
#include <format>
#include <fstream>
#include <iostream>

using namespace std::chrono_literals;

int main()
{
    auto p = std::filesystem::temp_directory_path() / "example.bin";
    std::ofstream{p.c_str()}.put('a'); // create file

    std::filesystem::file_time_type ftime = std::filesystem::last_write_time(p);
    std::cout << std::format("File write time is {}\n", ftime);

    // move file write time 1 hour to the future
    std::filesystem::last_write_time(p, ftime + 1h);

    // read back from the filesystem
    ftime = std::filesystem::last_write_time(p);
    std::cout << std::format("File write time is {}\n", ftime);

    std::filesystem::remove(p);
}
```

Possible output:

```text
File write time is 2023-09-04 19:33:24.702639224
File write time is 2023-09-04 20:33:24.702639224
```

### See also

- **last_write_time (C++17)** — gets or sets the time of the last data
  modification (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/file_time_type*
