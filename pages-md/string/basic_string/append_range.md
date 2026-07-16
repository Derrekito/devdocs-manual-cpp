# std::basic_string<CharT,Traits,Allocator>::append_range

```cpp
template< container-compatible-range<CharT> R >
constexpr std::basic_string& append_range( R&& rg );  // (since C++23)
```

Appends all characters from the range `rg`.

Equivalent to

```cpp
return append(std::basic_string( std::from_range, std​::​forward<R>(rg), get_allocator()));
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
    std::string head{"long long"};
    const auto tail = {' ', 'i', 'n', 't'};

#ifdef __cpp_lib_containers_ranges
    head.append_range(tail);
#else
    head.append(tail.begin(), tail.end());
#endif

    assert(head == "long long int");
}
```

### See also

- **append** — appends characters to the end (public member function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/append_range*
