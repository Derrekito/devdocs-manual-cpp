# std::filesystem::hash_value

```cpp
std::size_t hash_value( const std::filesystem::path& p ) noexcept;  // (since C++17)
```

### Parameters

- **p** — a `std::filesystem::path` object

### Return value

A hash value such that if for two paths, `p1 == p2` then `hash_value(p1) ==
hash_value(p2)`.

The return value is consistent with `std::hash`.

### Notes

Equality of two paths is determined by comparing each component separately, so,
for example `"a//b"` equals `"a/b"` and has the same `hash_value`.

`hash_value` originates from the Boost.filesystem library where it was used for
interoperability with boost.hash (which calls `hash_value` found by
argument-dependent lookup or `boost::hash_value` where available).

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
              << fs::hash_value(p) << " : " << p << '\n';
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

    // A hash function object to work with unordered_* containers:
    struct PathHash
    {
        std::size_t operator()(fs::path const& p) const noexcept
        {
            return fs::hash_value(p);
        }
    };
    std::unordered_set<fs::path, PathHash> dirs{
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

- **compare** — compares the lexical representations of two paths
  lexicographically (public member function)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (C++17)(C++17)(until C++20)(C++17)(until C++20)(C++17)(until
  C++20)(C++17)(until C++20)(C++17)(until C++20)(C++20)** — lexicographically
  compares two paths (function)
- **equivalent (C++17)** — checks whether two paths refer to the same file
  system object (function)
- **hash (C++11)** — hash function object (class template)
- **std::hash<std::filesystem::path> (C++17)** — hash support for
  `std::filesystem::path` (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/hash_value*
