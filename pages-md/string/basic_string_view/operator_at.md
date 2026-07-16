# std::basic_string_view<CharT,Traits>::operator[]

```cpp
constexpr const_reference operator[]( size_type pos ) const;  // (since C++17)
```

Returns a const reference to the character at specified location `pos`.

No bounds checking is performed: the behavior is undefined if `pos >= size()`.

### Parameters

- **pos** — position of the character to return

### Return value

Const reference to the requested character.

### Exceptions

Does not throw.

### Complexity

Constant.

### Notes

Unlike `std::basic_string::operator[]`,
`std::basic_string_view::operator[](size())` has undefined behavior instead of
returning `CharT()`.

### Example

```cpp
#include <iostream>
#include <string_view>

int main()
{
    std::string str = "Exemplar";
    std::string_view v = str;
    std::cout << v[2] << '\n';
//  v[2] = 'y'; // Error: cannot modify through a string view
    str[2] = 'y';
    std::cout << v[2] << '\n';
}
```

Output:

```text
e
y
```

### See also

- **at** — accesses the specified character with bounds checking (public member
  function)
- **operator[]** — accesses the specified character (public member function of
  `std::basic_string<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/operator_at*
