# break statement

Causes the enclosing for, range-for, while or do-while loop or switch statement
to terminate.

Used when it is otherwise awkward to terminate the loop using the condition
expression and conditional statements.

### Syntax

```text
attr ﻿(optional) break ;
```

- **attr** — (since C++11) any number of attributes

### Explanation

Appears only within the statement of a loop body (`while`, `do-while`, `for`) or
within the statement of a `switch`. After this statement the control is
transferred to the statement immediately following the enclosing loop or switch.
As with any block exit, all automatic storage objects declared in enclosing
compound statement or in the condition of a loop/switch are destroyed, in
reverse order of construction, before the execution of the first line following
the enclosing loop.

### Keywords

`break`

### Notes

A break statement cannot be used to break out of multiple nested loops. The goto
statement may be used for this purpose.

### Example

```cpp
#include <iostream>

int main()
{
    int i = 2;
    switch (i)
    {
        case 1: std::cout << "1";   // <---- maybe warning: fall through
        case 2: std::cout << "2";   // execution starts at this case label (+warning)
        case 3: std::cout << "3";   // <---- maybe warning: fall through
        case 4:                     // <---- maybe warning: fall through
        case 5: std::cout << "45";  //
                break;              // execution of subsequent statements is terminated
        case 6: std::cout << "6";
    }
    std::cout << '\n';

    for (char c = 'a'; c < 'c'; c++)
    {
        for (int i = 0; i < 5; i++)      // only this loop is affected by break
        {                                //
            if (i == 2)                  //
                break;                   //
            std::cout << c << i << ' ';  //
        }
    }
    std::cout << '\n';
}
```

Possible output:

```text
2345
a0 a1 b0 b1
```

### See also

- **`[[fallthrough]]`(C++17)** — indicates that the fall through from the
  previous case label is intentional and should not be diagnosed by a compiler
  that warns on fall-through (attribute specifier)

**C documentation for `break`**

---
*Source: https://en.cppreference.com/w/cpp/language/break*
