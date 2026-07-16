# std::basic_string<CharT,Traits,Allocator>::shrink_to_fit

```cpp
void shrink_to_fit();  // (until C++20)
constexpr void shrink_to_fit();  // (since C++20)
```

Requests the removal of unused capacity.

It is a non-binding request to reduce `capacity()` to `size()`. It depends on
the implementation if the request is fulfilled.

If (and only if) reallocation takes place, all pointers, references, and
iterators are invalidated.

### Parameters

(none)

### Return value

(none)

### Complexity

(unspecified)
*(until C++17)*

Linear in the size of the string.
*(since C++17)*

### Notes

In libstdc++, `shrink_to_fit()` is not available in C++98 mode.

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    std::string s;
    std::cout << "Size of std::string is " << sizeof s << " bytes\n"
        << "Default-constructed capacity is " << s.capacity()
        << " and size is " << s.size() << '\n';

    for (int i = 0; i < 42; i++)
        s.append(" 42 ");
    std::cout << "Capacity after 42 appends is " << s.capacity()
        << " and size is " << s.size() << '\n';

    s.clear();
    std::cout << "Capacity after clear() is " << s.capacity()
        << " and size is " << s.size() << '\n';

    s.shrink_to_fit();
    std::cout << "Capacity after shrink_to_fit() is " << s.capacity()
        << " and size is " << s.size() << '\n';
}
```

Possible output:

```text
GCC output:
Size of std::string is 32 bytes
Default-constructed capacity is 15 and size 0
Capacity after 42 appends is 240 and size 168
Capacity after clear() is 240 and size 0
Capacity after shrink_to_fit() is 15 and size 0

clang output (with -stdlib=libc++):
Size of std::string is 24 bytes
Default-constructed capacity is 22 and size is 0
Capacity after 42 appends is 191 and size is 168
Capacity after clear() is 191 and size is 0
Capacity after shrink_to_fit() is 22 and size is 0
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 755 | C++98 | `std::string` lacked explicit shrink-to-fit operations |
      provided

### See also

- **sizelength** — returns the number of characters (public member function)
- **capacity** — returns the number of characters that can be held in currently
  allocated storage (public member function)
- **resize** — changes the number of characters stored (public member function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/shrink_to_fit*
