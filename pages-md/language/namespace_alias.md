# Namespace aliases

Namespace aliases allow the programmer to define an alternate name for a
namespace.

They are commonly used as a convenient shortcut for long or deeply-nested
namespaces.

### Syntax

```text
namespace alias_name = ns_name;    (1)
namespace alias_name = ::ns_name;    (2)
namespace alias_name = nested_name::ns_name;    (3)
```

### Explanation

The new alias alias_name provides an alternate method of accessing ns_name.

alias_name must be a name not previously used. alias_name is valid for the
duration of the scope in which it is introduced.

### Example

```cpp
#include <iostream>

namespace foo
{
    namespace bar
    {
         namespace baz
         {
             int qux = 42;
         }
    }
}

namespace fbz = foo::bar::baz;

int main()
{
    std::cout << fbz::qux << '\n';
}
```

Output:

```text
42
```

### See also

- **namespace declaration** — identifies a namespace
- **type alias declaration (C++11)** — creates a synonym for a type

---
*Source: https://en.cppreference.com/w/cpp/language/namespace_alias*
