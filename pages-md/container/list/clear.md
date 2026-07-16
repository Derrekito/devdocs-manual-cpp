# std::list<T,Allocator>::clear

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
#include <list>

void print_info(std::string_view rem, const std::list<int>& v)
{
    std::cout << rem << "{ ";
    for (const auto& value : v)
        std::cout << value << ' ';
    std::cout << "}\n";
    std::cout << "Size=" << v.size() << '\n';
}

int main()
{
    std::list<int> container{1, 2, 3};
    print_info("Before clear: ", container);
    container.clear();
    print_info("After clear: ", container);
}
```

Output:

```text
Before clear: { 1 2 3 }
Size=3
After clear: { }
Size=0
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2231 | C++11 | complexity guarantee was mistakenly omitted in C++11 |
      complexity reaffirmed as linear

### See also

- **erase** — erases elements (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/list/clear*
