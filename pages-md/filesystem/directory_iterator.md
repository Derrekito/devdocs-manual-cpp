# std::filesystem::directory_iterator

`directory_iterator` (`std::filesystem`, C++17) walks the entries of
one directory — not its subdirectories — yielding a `directory_entry`
per file or folder. `.` and `..` are skipped automatically, and it's a
single-pass input iterator: iteration order is unspecified, so sort
the results yourself if order matters.

```cpp skip
for (const auto& entry : std::filesystem::directory_iterator{dir})   // throws
    use(entry);
std::error_code ec;
std::filesystem::directory_iterator it{dir, ec};   // non-throwing form
std::filesystem::directory_iterator{};             // the end iterator
```

### Member table

- **Member types** — `value_type` (`directory_entry`); `difference_type`
  (`std::ptrdiff_t`); `pointer` (`const directory_entry*`); `reference`
  (`const directory_entry&`); `iterator_category` (`input_iterator_tag`).
- **(constructor) / (destructor) / operator=**.
- **operator\* / operator->** — access the current `directory_entry`.
- **operator++ / increment()** — advance to the next entry;
  `increment(ec)` is the non-throwing form.
- **Non-member** — `begin`/`end` (C++17), enabling range-`for`.
  `operator==` (and, until C++20, `operator!=`) as required by
  LegacyInputIterator; whether `operator!=` is separately provided or
  synthesized, and whether equality is a member or non-member, is
  unspecified.

### Guarantees and costs

- LegacyInputIterator: single pass. Copies share iteration state, so a
  copied iterator doesn't give you an independent second pass.
- The default-constructed iterator is the end iterator; two end
  iterators always compare equal. Dereferencing or incrementing the
  end iterator is undefined behavior.
- Whether a file added or removed from the directory after the
  iterator was created shows up during iteration is unspecified.
- The constructor and the non-const advance operations cache whatever
  file attributes the underlying OS call already returned alongside
  each entry, without calling `directory_entry::refresh()` — so
  reading cached attributes off the entries as you iterate costs no
  extra system calls.
- Since C++20, `directory_iterator` additionally models
  `std::ranges::borrowed_range` and `std::ranges::view` (see
  Reference).

### Gotchas

- Never dereference or increment an end iterator — comparing against
  it is fine, using it further is UB.
- The default constructors/operations **throw** `filesystem_error` on
  a missing or unreadable directory; pass a trailing `std::error_code&`
  to the constructor and to `increment()` for the non-throwing forms
  when the path isn't trusted.
- It's single-pass: don't rely on iterating the same
  `directory_iterator` object twice — construct a fresh one (or
  collect entries into a container first).

### Example

```cpp
#include <algorithm>
#include <filesystem>
#include <fstream>
#include <iostream>
#include <vector>

namespace fs = std::filesystem;

int main()
{
    fs::path dir = fs::temp_directory_path() / "devdocs_diriter_demo";
    fs::remove_all(dir);
    fs::create_directory(dir);
    std::ofstream(dir / "b.txt") << "b";
    std::ofstream(dir / "a.txt") << "a";

    std::vector<std::string> names;
    for (const auto& entry : fs::directory_iterator{dir})
        names.push_back(entry.path().filename().string());

    std::sort(names.begin(), names.end());   // order is unspecified otherwise
    for (const auto& name : names)
        std::cout << name << '\n';

    fs::remove_all(dir);   // clean up
}
```

```text
a.txt
b.txt
```

### Reference

Full declaration:

```cpp skip
class directory_iterator;  // (since C++17)
```

```cpp skip
namespace std::ranges {
template<>
inline constexpr bool
    enable_borrowed_range<std::filesystem::directory_iterator> = true;
}  // (since C++20)
namespace std::ranges {
template<>
inline constexpr bool enable_view<std::filesystem::directory_iterator> = true;
}  // (since C++20)
```

Defect report LWG 3480 (applied to C++20): `directory_iterator` was
previously neither a `borrowed_range` nor a `view`; both
specializations above correct that.

### See also

- **recursive_directory_iterator (C++17)** — same, but also descends
  into subdirectories
- **directory_entry (C++17)** — the element type yielded by iteration
- **directory_options (C++17)** — flags controlling iteration (e.g.
  skipping permission-denied entries)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/directory_iterator*
