# std::filesystem::operator/(std::filesystem::path)

```cpp
friend path operator/( const path& lhs, const path& rhs );  // (since C++17)
```

Concatenates two path components using the preferred directory separator if
appropriate (see `operator/=` for details).

Effectively returns `path(lhs) /= rhs`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `std::filesystem::path` is
an associated class of the arguments. This prevents undesirable conversions in
the presence of a `using namespace std::filesystem;` *using-directive*.

### Parameters

- **lhs, rhs** — paths to concatenate

### Return value

The result of path concatenation.

### Example

```cpp
#include <filesystem>
#include <iostream>

int main()
{
#   if defined(_WIN32) // see e.g. stackoverflow.com/questions/142508

    std::filesystem::path p = "C:";

    std::cout << "\"C:\" / \"Users\" / \"batman\" == " << p / "Users" / "batman" << '\n';

#   else // __linux__ etc

    std::filesystem::path p = "/home";

    std::cout << "\"/home\" / \"tux\" / \".fonts\" == " << p / "tux" / ".fonts" << '\n';

#   endif
}
```

Possible output:

```text
Windows specific output:
"C:" / "Users" / "batman" == "C:Users\\batman"

Linux etc specific output:
"/home" / "tux" / ".fonts" == "/home/tux/.fonts"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3065 | C++17 | allowed concatenating everything convertible to `path` in
      the presence of a *using-directive* | made hidden friend

### See also

- **appendoperator/=** — appends elements to the path with a directory separator
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/operator_slash*
