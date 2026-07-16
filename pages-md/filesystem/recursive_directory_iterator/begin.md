# std::filesystem::begin(recursive_directory_iterator), std::filesystem::end(recursive_directory_iterator)

```cpp
recursive_directory_iterator begin( recursive_directory_iterator iter ) noexcept;  // (1) (since C++17)
recursive_directory_iterator end( recursive_directory_iterator ) noexcept;  // (2) (since C++17)
```

1) Returns `iter` unchanged.

2) Returns a default-constructed `recursive_directory_iterator`, which serves as
   the end iterator. The argument is ignored.

These non-member functions enable the use of `recursive_directory_iterator`s
with range-based for loops and make `recursive_directory_iterator` a `range`
type(since C++20).

### Parameters

- **iter** — a `recursive_directory_iterator`

### Return value

1) `iter` unchanged.

2) End iterator (default-constructed `recursive_directory_iterator`).

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
    fs::create_directories("sandbox/a/b");
    std::ofstream("sandbox/file1.txt");
    fs::create_symlink("a", "sandbox/syma");

    std::cout << "Print dir structure using OS specific command 'tree':\n";
    std::system("tree --noreport sandbox");

    std::cout << "\nPrint dir structure using directory iterator:\n";
    for (auto& p : fs::recursive_directory_iterator("sandbox"))
        std::cout << p << '\n';

    fs::remove_all("sandbox");
}
```

Possible output:

```text
Print dir structure using OS specific command 'tree':
sandbox
├── a
│   └── b
├── file1.txt
└── syma -> a

Print dir structure using directory iterator:
"sandbox/syma"
"sandbox/file1.txt"
"sandbox/a"
"sandbox/a/b"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3480 | C++17 | `end` took the argument by reference | takes the argument
      by value

### See also

-
  **begin(std::filesystem::directory_iterator)end(std::filesystem::directory_iterator)
  (C++17)** — range-based for loop support (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/recursive_directory_iterator/begin*
