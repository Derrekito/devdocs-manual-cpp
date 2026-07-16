# do-while loop

Executes a statement repeatedly, until the value of expression becomes false.
The test takes place after each iteration.

### Syntax

```text
attr ﻿(optional) do statement while ( expression ) ;
```

- **attr** — (since C++11) any number of attributes
- **expression** — any expression which is contextually convertible to bool.
  This expression is evaluated after each iteration, and if it yields `false`,
  the loop is exited.
- **statement** — any statement, typically a compound statement, which is the
  body of the loop

### Explanation

statement is always executed at least once, even if expression always yields
false. If it should not execute in this case, a while or for loop may be used.

If the execution of the loop needs to be terminated at some point, a break
statement can be used as terminating statement.

If the execution of the loop needs to be continued at the end of the loop body,
a continue statement can be used as shortcut.

### Notes

As part of the C++ forward progress guarantee, the behavior is undefined if a
loop that has no observable behavior (does not make calls to I/O functions,
access volatile objects, or perform atomic or synchronization operations) does
not terminate. Compilers are permitted to remove such loops.

### Keywords

`do`, `while`

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <string>

int main()
{
    int j = 2;
    do // compound statement is the loop body
    {
        j += 2;
        std::cout << j << ' ';
    }
    while (j < 9);
    std::cout << '\n';

    // common situation where do-while loop is used
    std::string s = "aba";
    std::sort(s.begin(), s.end());

    do std::cout << s << '\n'; // expression statement is the loop body
    while (std::next_permutation(s.begin(), s.end()));
}
```

Output:

```text
4 6 8 10
aab
aba
baa
```

### See also

**C documentation for `do-while`**

---
*Source: https://en.cppreference.com/w/cpp/language/do*
