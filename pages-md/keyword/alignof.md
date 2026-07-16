# C++ keyword: alignof (since C++11)

### Usage

- `alignof` operator (since C++11)

### Example

```cpp
#include <cstddef>
#include <iostream>

int main()
{
    std::cout << alignof(std::max_align_t) << '\n';
}
```

Possible output:

```text
16
```

---
*Source: https://en.cppreference.com/w/cpp/keyword/alignof*
