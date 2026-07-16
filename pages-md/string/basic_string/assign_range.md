# std::basic_string<CharT,Traits,Allocator>::assign_range

```cpp
template< container-compatible-range<CharT> R >
constexpr std::basic_string& assign_range( R&& rg );  // (since C++23)
```

Replaces the contents of the string with the values in the range `rg`.

Equivalent to

```cpp
return assign(
    std::basic_string(
        std::from_range,
        std​::​forward<R>(rg),
        get_allocator())
);
```

### Parameters

- **rg** — a container compatible range

### Return value

`*this`

### Complexity

Linear in size of `rg`.

### Exceptions

If the operation would result in `size() > max_size()`, throws
`std::length_error`.

If an exception is thrown for any reason, this function has no effect (strong
exception safety guarantee).

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_containers_ranges` | 202202L | (C++23) | member functions that
      accept container compatible range

### Example

```cpp
#include <cassert>
#include <string>

int main()
{
    const auto source = {'s', 'o', 'u', 'r', 'c', 'e'};
    std::string destination{"destination"};

#ifdef __cpp_lib_containers_ranges
    destination.assign_range(source);
#else
    destination.assign(source.begin(), source.end());
#endif

    assert(destination == "source");
}
```

### See also

- **assign** — assign characters to a string (public member function)
- **operator=** — assigns values to the string (public member function)
- **(constructor)** — constructs a `basic_string` (public member function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/assign_range*
