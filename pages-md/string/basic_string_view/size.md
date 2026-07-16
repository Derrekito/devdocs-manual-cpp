# std::basic_string_view<CharT,Traits>::size, std::basic_string_view<CharT,Traits>::length

```cpp
constexpr size_type size() const noexcept;  // (since C++17)
constexpr size_type length() const noexcept;  // (since C++17)
```

Returns the number of `CharT` elements in the view, i.e. `std::distance(begin(),
end())`.

### Parameters

(none)

### Return value

The number of `CharT` elements in the view.

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

- **empty** — checks whether the view is empty (public member function)
- **max_size** — returns the maximum number of characters (public member
  function)
- **sizelength** — returns the number of characters (public member function of
  `std::basic_string<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/size*
