# std::hash<std::filesystem::path>

```cpp
template<>
struct hash<std::filesystem::path>;  // (since C++17)
```

The template specialization of `std::hash` for `std::filesystem::path` allows
users to obtain hash values of `std::filesystem::path`.

The `operator()` of this specialization is `noexcept`. For every
`std::filesystem::path` value `p`, `std::hash<std::filesystem::path>{}(p)` is
equal to `std::filesystem::hash_value(p)`.

This specialization was absent from the C++17 standard publication, see LWG
issue 3657.

### Example

```cpp
#include <cassert>
#include <cstddef>
#include <filesystem>
#include <iomanip>
#include <iostream>
#include <unordered_set>
namespace fs = std::filesystem;

void show_hash(fs::path const& p)
{
    std::cout << std::hex << std::uppercase << std::setw(16)
              << std::hash<fs::path>{}(p) << " : " << p << '\n';
}

int main()
{
    auto tmp1 = fs::path{"/tmp"};
    auto tmp2 = fs::path{"/tmp/../tmp"};
    assert(!(tmp1 == tmp2));
    assert(fs::equivalent(tmp1, tmp2));
    show_hash(tmp1);
    show_hash(tmp2);

    for (auto s : {"/a///b", "/a//b", "/a/c", "...", "..", ".", ""})
        show_hash(s);

    std::unordered_set<fs::path, std::hash<fs::path>> dirs{
        "/bin", "/bin", "/lib", "/lib", "/opt", "/opt", "/tmp", "/tmp/../tmp"};
    for (fs::path const& p : dirs)
        std::cout << p << ' ';
    std::cout << '\n';
}
```

Possible output:

```text
6050C47ADB62DFE5 : "/tmp"
62795A58B69AD90A : "/tmp/../tmp"
FF302110C9991974 : "/a///b"
FF302110C9991974 : "/a//b"
FD6167277915D464 : "/a/c"
C42040F82CD8B542 : "..."
D2D30154E0B78BBC : ".."
D18C722215ED0530 : "."
               0 : ""
"/tmp/../tmp" "/opt" "/lib" "/tmp" "/bin"
```

### See also

- **hash (C++11)** — hash function object (class template)
- **hash_value (C++17)** — calculates a hash value for a path object (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/hash*
