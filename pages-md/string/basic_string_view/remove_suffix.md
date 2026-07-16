# std::basic_string_view<CharT,Traits>::remove_suffix

```cpp
constexpr void remove_suffix( size_type n );  // (since C++17)
```

Moves the end of the view back by `n` characters.

The behavior is undefined if `n > size()`.

### Parameters

- **n** — number of characters to remove from the end of the view

### Return value

(none)

### Complexity

Constant.

### Example

```cpp
#include <iostream>
#include <string_view>

int main()
{
    char arr[] = {'a', 'b', 'c', 'd', '\0', '\0', '\0'};
    std::string_view v(arr, sizeof arr);
    auto trim_pos = v.find('\0');
    if (trim_pos != v.npos)
        v.remove_suffix(v.size() - trim_pos);
    std::cout << "Array: '" << arr << "', size=" << sizeof arr << '\n'
              << "View : '" << v << "', size=" << v.size() << '\n';
}
```

Output:

```text
Array: 'abcd', size=7
View : 'abcd', size=4
```

### See also

- **remove_prefix** — shrinks the view by moving its start forward (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/remove_suffix*
