# continue statement

Causes the remaining portion of the enclosing for, range-for, while or do-while
loop body to be skipped.

Used when it is otherwise awkward to ignore the remaining portion of the loop
using conditional statements.

### Syntax

```text
attr ﻿(optional) continue ;
```

### Explanation

The `continue` statement causes a jump, as if by goto to the end of the loop
body (it may only appear within the loop body of for, range-for, while, and
do-while loops).

More precisely,

For while loop, it acts as

```cpp
while (/* ... */)
{
   // ...
   continue; // acts as goto contin;
   // ...
   contin:;
}
```

For do-while loop, it acts as:

```cpp
do
{
    // ...
    continue; // acts as goto contin;
    // ...
    contin:;
} while (/* ... */);
```

For for and range-for loop, it acts as:

```cpp
for (/* ... */)
{
    // ...
    continue; // acts as goto contin;
    // ...
    contin:;
}
```

### Keywords

`continue`

### Example

```cpp
#include <iostream>

int main()
{
    for (int i = 0; i < 10; ++i)
    {
        if (i != 5)
            continue;
        std::cout << i << ' ';      // this statement is skipped each time i != 5
    }
    std::cout << '\n';

    for (int j = 0; 2 != j; ++j)
        for (int k = 0; k < 5; ++k) // only this loop is affected by continue
        {
            if (k == 3)
                continue;
            // this statement is skipped each time k == 3:
            std::cout << '(' << j << ',' << k << ") ";
        }
    std::cout << '\n';
}
```

Output:

```text
5
(0,0) (0,1) (0,2) (0,4) (1,0) (1,1) (1,2) (1,4)
```

### See also

**C documentation for `continue`**

---
*Source: https://en.cppreference.com/w/cpp/language/continue*
