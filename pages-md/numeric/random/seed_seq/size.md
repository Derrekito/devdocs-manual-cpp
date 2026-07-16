# std::seed_seq::size

```cpp
std::size_t size() const noexcept;  // (since C++11)
```

Returns the size of the stored initial seed sequence.

### Parameters

(none)

### Return value

The size of the private container that was populated at construction time.

### Complexity

Constant time.

### Example

```cpp
#include <iostream>
#include <random>

int main()
{
    std::seed_seq s1 = {-1, 0, 1};
    std::cout << s1.size() << '\n';
}
```

Output:

```text
3
```

### Defect report

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2440 | C++11 | `seed_seq::size` was not required to be noexcept | required

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/seed_seq/size*
