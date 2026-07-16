# std::filesystem::directory_entry::file_size

```cpp
std::uintmax_t file_size() const;  // (1) (since C++17)
std::uintmax_t file_size( std::error_code& ec ) const noexcept;  // (2) (since C++17)
```

If the file size is cached in this `directory_entry`, returns the cached value.
Otherwise, returns:

1) `std::filesystem::file_size(path())`,

2) `std::filesystem::file_size(path(), ec)`.

### Parameters

- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

The size of the referred-to filesystem object.

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

1) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `p` as the first path argument and the OS error code as the
   error code argument.

2) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Example

Prints the list of files in a given directory alongside with their sizes in
human readable form.

```cpp
#include <cmath>
#include <cstdint>
#include <filesystem>
#include <iostream>

struct HumanReadable
{
    std::uintmax_t size{};

    template<typename Os> friend Os& operator<<(Os& os, HumanReadable hr)
    {
        int i{};
        double mantissa = hr.size;
        for (; mantissa >= 1024.0; mantissa /= 1024.0, ++i)
        {}
        os << std::ceil(mantissa * 10.0) / 10.0 << i["BKMGTPE"];
        return i ? os << "B (" << hr.size << ')' : os;
    }
};

int main(int argc, const char* argv[])
{
    const auto dir = argc == 2 ? std::filesystem::path{argv[1]}
                               : std::filesystem::current_path();

    for (std::filesystem::directory_entry const& entry :
         std::filesystem::directory_iterator(dir))
        if (entry.is_regular_file())
            std::cout << entry.path().filename() << " size: "
                      << HumanReadable{entry.file_size()} << '\n';
}
```

Possible output:

```text
"boost_1_73_0.tar.bz2" size: 104.2MB (109247910)
"CppCon 2018 - Jon Kalb “Copy Elision”.mp4" size: 15.7MB (16411990)
"cppreference-doc-20190607.tar.xz" size: 6.3MB (6531336)
"hana.hpp" size: 6.7KB (6807)
```

### See also

- **file_size (C++17)** — returns the size of a file (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/directory_entry/file_size*
