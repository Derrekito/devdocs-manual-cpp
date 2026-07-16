# std::unordered_set<Key,Hash,KeyEqual,Allocator>::clear

```cpp
void clear() noexcept;  // (since C++11)
```

Erases all elements from the container. After this call, `size()` returns zero.

Invalidates any references, pointers, or iterators referring to contained
elements. May also invalidate past-the-end iterators.

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
#include <unordered_set>

void print_info(std::string_view rem, const std::unordered_set<int>& v)
{
    std::cout << rem << "{ ";
    for (const auto& value : v)
        std::cout << value << ' ';
    std::cout << "}\n";
    std::cout << "Size=" << v.size() << '\n';
}

int main()
{
    std::unordered_set<int> container{1, 2, 3};
    print_info("Before clear: ", container);
    container.clear();
    print_info("After clear: ", container);
}
```

Possible output:

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
  LWG 2550 | C++11 | for unordered associative containers, unclear if complexity
      is linear in the number of elements or buckets | clarified that it's
      linear in the number of elements

### See also

- **erase** — erases elements (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_set/clear*
