# C++ attribute: optimize_for_synchronized (TM TS)

Indicates that the function definition should be optimized for invocation from a
synchronized statement.

### Syntax

```text
[[optimize_for_synchronized]]
```

### Explanation

Applies to the name being declared in a function declaration, which must be the
first declaration of the function.

Indicates that the function definition should be optimized for invocation from a
synchronized statement. In particular, it avoids serializing synchronized blocks
that make a call to a function that is transaction-safe for the majority of
calls, but not for all calls.

### Example

---
*Source: https://en.cppreference.com/w/cpp/language/attributes/optimize_for_synchronized*
