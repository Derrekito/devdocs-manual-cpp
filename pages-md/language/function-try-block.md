# Function-try-block

Establishes an exception handler around the body of a function.

### Syntax

The function-try-block is one of the alternative syntax forms for function-body,
which is a part of function definition.

```text
try ctor-initializer ﻿(optional) compound-statement handler-sequence
```

- **ctor-initializer** — member initializer list, only allowed in constructors
- **compound-statement** — the brace-enclosed sequence of statements that
  constitutes the body of a function
- **handler-sequence** — sequence of one or more catch-clauses

### Explanation

A *function-try-block* associates a sequence of catch clauses with the entire
function body, and with the member initializer list (if used in a constructor)
as well. Every exception thrown from any statement in the function body, or (for
constructors) from any member or base constructor, or (for destructors) from any
member or base destructor, transfers control to the handler-sequence the same
way an exception thrown in a regular try block would.

```cpp
#include <iostream>
#include <string>

struct S
{
    std::string m;

    S(const std::string& str, int idx)
    try : m(str, idx)
    {
        std::cout << "S(" << str << ", " << idx << ") constructed, m = " << m << '\n';
    }
    catch(const std::exception& e)
    {
        std::cout << "S(" << str << ", " << idx << ") failed: " << e.what() << '\n';
    } // implicit "throw;" here for constructor
};

int main()
{
    S s1{"ABC", 1}; // does not throw (index is in bounds)

    try
    {
        S s2{"ABC", 4}; // throws (out of bounds)
    }
    catch (std::exception& e)
    {
        std::cout << "S s2... raised an exception: " << e.what() << '\n';
    }
}
```

Before any catch clauses of a function-try-block on a constructor are entered,
all fully-constructed members and bases have already been destroyed.

If the function-try-block is on a delegating constructor, which called a
non-delegating constructor that completed successfully, but then the body of the
delegating constructor throws, the destructor of this object will be completed
before any catch clauses of the function-try-block are entered.
*(since C++11)*

Before any catch clauses of a function-try-block on a destructor are entered,
all bases and non-variant members have already been destroyed.

The behavior is undefined if the catch-clause of a function-try-block used on a
constructor or a destructor accesses a base or a non-static member of the
object.

Every catch-clause in the function-try-block for a constructor must terminate by
throwing an exception. If the control reaches the end of such handler, the
current exception is automatically rethrown as if by `throw;`. The return
statement is not allowed in any catch clause of a constructor's
function-try-block.

Reaching the end of a catch clause for a function-try-block on a destructor also
automatically rethrows the current exception as if by `throw;`, but a return
statement is allowed.

For all other functions, reaching the end of a catch clause is equivalent to
`return;` if the function's return type is (possibly cv-qualified) `void`,
otherwise the behavior is undefined.

### Notes

The primary purpose of function-try-blocks is to respond to an exception thrown
from the member initializer list in a constructor by logging and rethrowing,
modifying the exception object and rethrowing, throwing a different exception
instead, or terminating the program. They are rarely used with destructors or
with regular functions.

Function-try-block does not catch the exceptions thrown by the copy/move
constructors and the destructors of the function parameters passed by value:
those exceptions are thrown in context of the caller.

Function-try-block of the top-level function of a thread does not catch the
exceptions thrown from the constructors and destructors of thread-local objects
(except for the constructors of function-scoped thread-locals).
*(since C++11)*

Likewise, function-try-block of the main() function does not catch the
exceptions thrown from the constructors and destructors of static objects
(except for the constructors of function-local statics).

The scope and lifetime of the function parameters (but not any objects declared
in the function itself), extend to the end of the handler-sequence.

```cpp
int f(int n = 2) try
{
    ++n; // increments the function parameter
    throw n;
}
catch(...)
{
    ++n; // n is in scope and still refers to the function parameter
    assert(n == 4);
    return n;
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 1167 | C++98 | it was unspecified whether a function-try-block on a
      destructor will catch exceptions from a base or member destructor | such
      exceptions are caught

---
*Source: https://en.cppreference.com/w/cpp/language/function-try-block*
