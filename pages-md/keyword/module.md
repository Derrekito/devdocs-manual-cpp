# C++ keyword: module (since C++20)

### Usage

- `module` declaration: declares that the current translation unit is a *module
  unit* (since C++20)
- Starts a *global module fragment* of a module unit (since C++20)
- Starts a *private module fragment* of a module unit (since C++20)

### Example

```cpp
module;            // starts a global module fragment

#include <string>

export module foo; // ends a global module fragment
                   // declares the primary module interface unit for named module 'foo'
                   // starts a module unit purview

export std::string f();

module :private;   // ends the portion of the module interface unit that
                   // can affect the behavior of other translation units
                   // starts a private module fragment

std::string f()
{
    return "foo";
}
```

---
*Source: https://en.cppreference.com/w/cpp/keyword/module*
