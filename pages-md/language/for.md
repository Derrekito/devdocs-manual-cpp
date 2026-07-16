# for loop

Executes init-statement once, then executes statement and iteration-expression
repeatedly, until the value of condition becomes false. The test takes place
before each iteration.

### Syntax

```text
formal syntax:
attr ﻿(optional) for ( init-statement condition ﻿(optional) ; iteration-expression ﻿(optional) ) statement
informal syntax:
attr ﻿(optional) for ( declaration-or-expression ﻿(optional) ; condition ﻿(optional) ; expression ﻿(optional) ) statement
```

- **attr** — (since C++11) any number of attributes.
- **init-statement** — one of an expression statement (which may be a *null
  statement* "`;`"). a simple declaration, typically a declaration of a loop
  counter variable with initializer, but it may declare arbitrary many variables
  or structured bindings(since C++17). an alias declaration. (since C++23) Note
  that any init-statement must end with a semicolon `;`, which is why it is
  often described informally as an expression or a declaration followed by a
  semicolon.
- **an alias declaration.** — (since C++23)
- **condition** — either an expression which is contextually convertible to
  bool. This expression is evaluated before each iteration, and if its value
  converts to `false`, the loop is exited. a declaration of a single variable
  with a brace-or-equals initializer. The initializer is evaluated before each
  iteration, and if the value of the declared variable converts to `false`, the
  loop is exited.
- **iteration-expression** — any expression, which is executed after every
  iteration of the loop and before re-evaluating condition. Typically, this is
  the expression that increments the loop counter.
- **statement** — any statement, typically a compound statement, which is the
  body of the loop.

### Explanation

The above syntax produces code equivalent to:

```text
{ init-statement while ( condition ) { statement iteration-expression ; } }
```

Except that

1) The scope of init-statement and the scope of condition are the same.

2) The scope of statement and the scope of iteration-expression are disjoint and
   nested within the scope of init-statement and condition.

3) continue in statement will execute iteration-expression.

4) Empty condition is equivalent to `while (true)`.

If the execution of the loop needs to be terminated at some point, break
statement can be used as terminating statement.

If the execution of the loop needs to be continued at the end of the loop body,
continue statement can be used as shortcut.

As is the case with while loop, if statement is a single statement (not a
compound statement), the scope of variables declared in it is limited to the
loop body as if it was a compound statement.

```cpp
for (;;)
    int n;
// n goes out of scope
```

### Keywords

`for`

### Notes

As part of the C++ forward progress guarantee, the behavior is undefined if a
loop that has no observable behavior (does not make calls to I/O functions,
access volatile objects, or perform atomic or synchronization operations) does
not terminate. Compilers are permitted to remove such loops. While in C names
declared in the scope of init-statement and condition can be shadowed in the
scope of statement, it is forbidden in C++:

```cpp
for (int i = 0;;)
{
    long i = 1;   // valid C, invalid C++
    // ...
}
```

### Example

```cpp
#include <iostream>
#include <vector>

int main()
{
    std::cout << "1) typical loop with a single statement as the body:\n";
    for (int i = 0; i < 10; ++i)
        std::cout << i << ' ';

    std::cout << "\n\n" "2) init-statement can declare multiple names, as\n"
                 "long as they can use the same decl-specifier-seq:\n";
    for (int i = 0, *p = &i; i < 9; i += 2)
        std::cout << i << ':' << *p << ' ';

    std::cout << "\n\n" "3) condition may be a declaration:\n";
    char cstr[] = "Hello";
    for (int n = 0; char c = cstr[n]; ++n)
        std::cout << c;

    std::cout << "\n\n" "4) init-statement can use the auto type specifier:\n";
    std::vector<int> v = {3, 1, 4, 1, 5, 9};
    for (auto iter = v.begin(); iter != v.end(); ++iter)
        std::cout << *iter << ' ';

    std::cout << "\n\n" "5) init-statement can be an expression:\n";
    int n = 0;
    for (std::cout << "Loop start\n";
         std::cout << "Loop test\n";
         std::cout << "Iteration " << ++n << '\n')
    {
        if (n > 1)
            break;
    }

    std::cout << "\n" "6) constructors and destructors of objects created\n"
                 "in the loop's body are called per each iteration:\n";
    struct S
    {
        S(int x, int y) { std::cout << "S::S(" << x << ", " << y << "); "; }
        ~S() { std::cout << "S::~S()\n"; }
    };
    for (int i{0}, j{5}; i < j; ++i, --j)
        S s{i, j};

    std::cout << "\n" "7) init-statement can use structured bindings:\n";
    long arr[]{1, 3, 7};
    for (auto [i, j, k] = arr; i + j < k; ++i)
        std::cout << i + j << ' ';
    std::cout << '\n';
}
```

Output:

```text
1) typical loop with a single statement as the body:
0 1 2 3 4 5 6 7 8 9

2) init-statement can declare multiple names, as
long as they can use the same decl-specifier-seq:
0:0 2:2 4:4 6:6 8:8

3) condition may be a declaration:
Hello

4) init-statement can use the auto type specifier:
3 1 4 1 5 9

5) init-statement can be an expression:
Loop start
Loop test
Iteration 1
Loop test
Iteration 2
Loop test

6) constructors and destructors of objects created
in the loop's body are called per each iteration:
S::S(0, 5); S::~S()
S::S(1, 4); S::~S()
S::S(2, 3); S::~S()

7) init-statement can use structured bindings:
4 5 6
```

### See also

- **range-`for` loop(C++11)** — executes loop over range

**C documentation for `for`**

---
*Source: https://en.cppreference.com/w/cpp/language/for*
