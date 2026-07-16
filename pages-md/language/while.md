# while loop

Executes a statement repeatedly, until the value of condition becomes `false`.
The test takes place before each iteration.

### Syntax

```text
attr ﻿(optional) while ( condition ) statement
```

- **attr** — (since C++11) any number of attributes
- **condition** — any expression which is contextually convertible to bool or a
  declaration of a single variable with a brace-or-equals initializer. This
  expression is evaluated before each iteration, and if it yields `false`, the
  loop is exited. If this is a declaration, the initializer is evaluated before
  each iteration, and if the value of the declared variable converts to `false`,
  the loop is exited.
- **statement** — any statement, typically a compound statement, which is the
  body of the loop

### Explanation

Whether statement is a compound statement or not, it always introduces a block
scope. Variables declared in it are only visible in the loop body, in other
words,

```cpp
while (--x >= 0)
    int i;
// i goes out of scope
```

is the same as

```cpp
while (--x >= 0)
{
    int i;
} // i goes out of scope
```

If condition is a declaration such as `T t = x`, the declared variable is only
in scope in the body of the loop, and is destroyed and recreated on every
iteration, in other words, such while loop is equivalent to

```cpp
label:
{ // start of condition scope
    T t = x;
    if (t)
    {
        statement
        goto label; // calls the destructor of t
    }
}
```

If the execution of the loop needs to be terminated at some point, break
statement can be used as terminating statement.

If the execution of the loop needs to be continued at the end of the loop body,
continue statement can be used as shortcut.

### Notes

As part of the C++ forward progress guarantee, the behavior is undefined if a
loop that has no observable behavior (does not make calls to I/O functions,
access volatile objects, or perform atomic or synchronization operations) does
not terminate. Compilers are permitted to remove such loops.

### Keywords

`while`

### Example

```cpp
#include <iostream>

int main()
{
    // while loop with a single statement
    int i = 0;
    while (i < 10)
         i++;
    std::cout << i << '\n';

    // while loop with a compound statement
    int j = 2;
    while (j < 9)
    {
        std::cout << j << ' ';
        j += 2;
    }
    std::cout << '\n';

   // while loop with a declaration condition
   char cstr[] = "Hello";
   int k = 0;
   while (char c = cstr[k++])
       std::cout << c;
   std::cout << '\n';
}
```

Output:

```text
10
2 4 6 8
Hello
```

### See also

**C documentation for `while`**

---
*Source: https://en.cppreference.com/w/cpp/language/while*
