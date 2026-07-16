# C++ attribute: fallthrough (since C++17)

Indicates that the fall through from the previous case label is intentional and
should not be diagnosed by a compiler that warns on fallthrough.

### Syntax

```text
[[fallthrough]]
```

### Explanation

May only be applied to a null statement to create a *fallthrough statement*
(`[[fallthrough]];`).

A fallthrough statement may only be used in a switch statement, where the next
statement to be executed is a statement with a case or default label for that
switch statement. If the fallthrough statement is inside a loop, the next
(labeled) statement must be part of the same iteration of that loop.

### Example

```cpp
void f(int n)
{
    void g(), h(), i();

    switch (n)
    {
        case 1:
        case 2:
            g();
            [[fallthrough]];
        case 3: // no warning on fallthrough
            h();
        case 4: // compiler may warn on fallthrough
            if (n < 3)
            {
                i();
                [[fallthrough]]; // OK
            }
            else
            {
                return;
            }
        case 5:
            while (false)
            {
                [[fallthrough]]; // ill-formed: next statement is not
                                 //             part of the same iteration
            }
        case 6:
            [[fallthrough]]; // ill-formed, no subsequent case or default label
    }
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 2406 | C++17 | `[[fallthrough]]` could appear in a loop nested inside the
      target switch statement | prohibited

### See also

**C documentation for `fallthrough`**

---
*Source: https://en.cppreference.com/w/cpp/language/attributes/fallthrough*
