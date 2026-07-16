# C++ keyword: and_eq

### Usage

- alternative operators: as an alternative for `&=`

### Example

```cpp
#include <bitset>
#include <iostream>

int main()
{
    std::bitset<4> mask("1100");
    std::bitset<4> val("0111");
    val and_eq mask;
    std::cout << val << '\n';
}
```

Output:

```text
0100
```

---
*Source: https://en.cppreference.com/w/cpp/keyword/and_eq*
