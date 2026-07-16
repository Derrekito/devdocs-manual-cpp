# std::basic_string<CharT,Traits,Allocator>::at

```cpp
reference at( size_type pos );  // (until C++20)
constexpr reference at( size_type pos );  // (since C++20)
const_reference at( size_type pos ) const;  // (until C++20)
constexpr const_reference at( size_type pos ) const;  // (since C++20)
```

Returns a reference to the character at specified location `pos`. Bounds
checking is performed, exception of type `std::out_of_range` will be thrown on
invalid access.

### Parameters

- **pos** — position of the character to return

### Return value

Reference to the requested character.

### Exceptions

Throws `std::out_of_range` if `pos >= size()`.

If an exception is thrown for any reason, these functions have no effect (strong
exception safety guarantee).

### Complexity

Constant.

### Example

```cpp
#include <iostream>
#include <stdexcept>
#include <string>

int main()
{
    std::string s("message"); // for capacity

    s = "abc";
    s.at(2) = 'x'; // OK
    std::cout << s << '\n';

    std::cout << "string size = " << s.size() << '\n';
    std::cout << "string capacity = " << s.capacity() << '\n';

    try
    {
        // This will throw since the requested offset is greater than the current size.
        s.at(3) = 'x';
    }
    catch (std::out_of_range const& exc)
    {
        std::cout << exc.what() << '\n';
    }
}
```

Possible output:

```text
abx
string size = 3
string capacity = 7
basic_string::at
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 847 | C++98 | there was no exception safety guarantee | added strong
      exception safety guarantee

### See also

- **operator[]** — accesses the specified character (public member function)
- **at** — accesses the specified character with bounds checking (public member
  function of `std::basic_string_view<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/at*
