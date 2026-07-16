# std::multimap<Key,T,Compare,Allocator>::clear

```cpp
void clear();  // (until C++11)
void clear() noexcept;  // (since C++11)
```

Erases all elements from the container. After this call, `size()` returns zero.

Invalidates any references, pointers, or iterators referring to contained
elements. Any past-the-end iterator remains valid.

### Parameters

(none)

### Return value

(none)

### Complexity

Linear in the size of the container, i.e., the number of elements.

### Example

```cpp
#include <iostream>
#include <string_view>
#include <map>

void print_info(std::string_view rem, const std::multimap<int, char>& v)
{
    std::cout << rem << "{ ";
    for (const auto& [key, value] : v)
        std::cout << '[' << key << "]:" << value << ' ';
    std::cout << "}\n";
    std::cout << "Size=" << v.size() << '\n';
}

int main()
{
    std::multimap<int, char> container{{1, 'x'}, {2, 'y'}, {3, 'z'}};
    print_info("Before clear: ", container);
    container.clear();
    print_info("After clear: ", container);
}
```

Output:

```text
Before clear: { [1]:x [2]:y [3]:z }
Size=3
After clear: { }
Size=0
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 224 | C++98 | the complexity was `log(size()) + N`, but `N` was not
      defined | corrected to 'linear in `size()`'

### See also

- **erase** — erases elements (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/multimap/clear*
