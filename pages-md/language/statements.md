# Statements

Statements are fragments of the C++ program that are executed in sequence. The
body of any function is a sequence of statements. For example:

```cpp
int main()
{
    int n = 1;                        // declaration statement
    n = n + 1;                        // expression statement
    std::cout << "n = " << n << '\n'; // expression statement
    return 0;                         // return statement
}
```

C++ includes the following types of statements:

1) labeled statements;

2) expression statements;

3) compound statements;

4) selection statements;

5) iteration statements;

6) jump statements;

7) declaration statements;

8) try blocks;

9) atomic and synchronized blocks (TM TS).

### Labeled statements

A labeled statement labels a statement for control flow purposes.

```text
label statement
```

- **label** — the label applied to the statement (defined below)
- **statement** — the statement which the label applies to, it can be a labeled
  statement itself, allowing multiple labels

label is defined as

```text
attr ﻿(optional) identifier :    (1)
attr ﻿(optional) case constexpr :    (2)
attr ﻿(optional) default:    (3)
```

1) target for goto;

2) case label in a switch statement;

3) default label in a switch statement.

An attribute sequence attr may appear just at the beginning of the label (in
which case it applies to the label), or just before any statement itself, in
which case it applies to the entire statement.
*(since C++11)*

A label with an identifier declared inside a function matches all goto
statements with the same identifier in that function, in all nested blocks,
before and after its own declaration.

Two labels in a function must not have the same identifier.

Besides being added to a statement, labels can also be used anywhere in compound
statements.
*(since C++23)*

Labels are not found by unqualified lookup: a label can have the same name as
any other entity in the program.

```cpp
void f()
{
    {
        goto label; // label in scope even though declared later
        label:      // label can appear at the end of a block standalone since C++23
    }
    goto label; // label ignores block scope
}

void g()
{
    goto label; // error: label not in scope in g()
}
```

### Expression statements

An expression statement is an expression followed by a semicolon.

```text
attr ﻿(optional) expression ﻿(optional) ;
```

- **attr** — (since C++11) optional sequence of any number of attributes
- **expression** — an expression

Most statements in a typical C++ program are expression statements, such as
assignments or function calls.

An expression statement without an expression is called a *null statement*. It
is often used to provide an empty body to a for or while loop. It can also be
used to carry a label in the end of a compound statement.(until C++23)

### Compound statements

A compound statement or *block* groups a sequence of statements into a single
statement.

```text
attr ﻿(optional) { statement... ﻿(optional) label... ﻿(optional)(since C++23) }
```

When one statement is expected, but multiple statements need to be executed in
sequence (for example, in an if statement or a loop), a compound statement may
be used:

```cpp
if (x > 5)          // start of if statement
{                   // start of block
    int n = 1;      // declaration statement
    std::cout << n; // expression statement
}                   // end of block, end of if statement
```

Each compound statement introduces its own block scope; variables declared
inside a block are destroyed at the closing brace in reverse order:

```cpp
int main()
{ // start of outer block
    {                                // start of inner block
        std::ofstream f("test.txt"); // declaration statement
        f << "abc\n";                // expression statement
    }                                // end of inner block, f is flushed and closed
    std::ifstream f("test.txt"); // declaration statement
    std::string str;             // declaration statement
    f >> str;                    // expression statement
} // end of outer block, str is destroyed, f is closed
```

A label at the end of a compound statement is treated as if it were followed by
a null statement.
*(since C++23)*

### Selection statements

A selection statement chooses between one of several control flows.

```text
attr ﻿(optional) if constexpr(optional) ( init-statement ﻿(optional) condition ) statement    (1)
attr ﻿(optional) if constexpr(optional) ( init-statement ﻿(optional) condition ) statement else statement    (2)
attr ﻿(optional) switch ( init-statement ﻿(optional) condition ) statement    (3)
attr ﻿(optional) if !(optional) consteval compound-statement    (4)    (since C++23)
attr ﻿(optional) if !(optional) consteval compound-statement else statement    (5)    (since C++23)
```

1) if statement;

2) if statement with an else clause;

3) switch statement;

4) consteval if statement;

5) consteval if statement with an else clause.

### Iteration statements

An iteration statement repeatedly executes some code.

```text
attr ﻿(optional) while ( condition ) statement    (1)
attr ﻿(optional) do statement while ( expression ) ;    (2)
attr ﻿(optional) for ( init-statement condition ﻿(optional) ; expression ﻿(optional) ) statement    (3)
attr ﻿(optional) for ( init-statement ﻿(optional)(since C++20) for-range-decl : for-range-init ) statement    (4)    (since C++11)
```

1) while loop;

2) do-while loop;

3) for loop;

4) range for loop.

### Jump statements

A jump statement unconditionally transfers control flow.

```text
attr ﻿(optional) break;    (1)
attr ﻿(optional) continue;    (2)
attr ﻿(optional) return expression ﻿(optional) ;    (3)
attr ﻿(optional) return braced-init-list ;    (4)    (since C++11)
attr ﻿(optional) goto identifier ;    (5)
```

1) break statement;

2) continue statement;

3) return statement with an optional expression;

4) return statement using list initialization;

5) goto statement.

Note: for all jump statements, transfer out of a loop, out of a block, or back
past an initialized variable with automatic storage duration involves the
destruction of objects with automatic storage duration that are in scope at the
point transferred from but not at the point transferred to. If multiple objects
were initialized, the order of destruction is the opposite of the order of
initialization.

### Declaration statements

A declaration statement introduces one or more identifiers into a block.

```text
block-declaration    (1)
```

1) see Declarations and Initialization for details.

### Try blocks

A try block catches exceptions thrown when executing other statements.

```text
attr ﻿(optional) try compound-statement handler-sequence    (1)
```

1) see try/catch for details.

### Atomic and synchronized blocks
An atomic and synchronized block provides transactional memory.
```text
synchronized compound-statement    (1)    (TM TS)
atomic_noexcept compound-statement    (2)    (TM TS)
atomic_cancel compound-statement    (3)    (TM TS)
atomic_commit compound-statement    (4)    (TM TS)
```
1) synchronized block, executed in single total order with all synchronized
blocks;
2) atomic block that aborts on exceptions;
3) atomic block that rolls back on exceptions;
4) atomic block that commits on exceptions.
*(TM TS)*

### See also

**C documentation for Statements**

---
*Source: https://en.cppreference.com/w/cpp/language/statements*
