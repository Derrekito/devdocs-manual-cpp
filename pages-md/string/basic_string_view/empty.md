# std::basic_string_view<CharT,Traits>::empty

```cpp
constexpr bool empty() const noexcept;  // (since C++17) (until C++20)
[[nodiscard]] constexpr bool empty() const noexcept;  // (since C++20)
```

Checks if the view has no characters, i.e. whether `size()` `==` `​0​`.

### Parameters

(none)

### Return value

`true` if the view is empty, `false` otherwise.

### Complexity

Constant.

### Example

```cpp
#include <iostream>
#include <string_view>

// Print a string surrounded by single quotes, its
// length and whether it is considered empty.
void check_string(std::string_view ref)
{
    std::cout << std::boolalpha
              << "'" << ref << "' has " << ref.size()
              << " character(s); emptiness: " << ref.empty() << '\n';
}

int main(int argc, char **argv)
{
    // An empty string
    check_string("");

    // Almost always not empty: argv[0]
    if (argc > 0)
        check_string(argv[0]);
}
```

Possible output:

```text
'' has 0 character(s); emptiness: true
'./a.out' has 7 character(s); emptiness: false
```

### See also

- **sizelength** — returns the number of characters (public member function)
- **max_size** — returns the maximum number of characters (public member
  function)
- **sizessize (C++17)(C++20)** — returns the size of a container or array
  (function template)
- **empty (C++17)** — checks whether the container is empty (function template)
- **empty** — checks whether the string is empty (public member function of
  `std::basic_string<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/empty*
