# std::type_index::name

```cpp
const char* name() const noexcept;  // (since C++11)
```

Returns the name of the associated `std::type_info` object. Equivalent to
calling `std::type_info::name()` directly.

### Parameters

(none)

### Return value

The name of the associated `std::type_info` object.

### Example

```cpp
#include <iostream>
#include <typeindex>
int main() {
    std::cout << std::type_index(typeid(std::cout)).name();
}
```

Possible output:

```text
NSt3__113basic_ostreamIcNS_11char_traitsIcEEEE
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2144 | C++11 | `type_index::name` was not required to be noexcept |
      required

---
*Source: https://en.cppreference.com/w/cpp/types/type_index/name*
