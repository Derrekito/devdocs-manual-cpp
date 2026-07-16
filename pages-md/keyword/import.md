# C++ keyword: import (since C++20)

### Usage

- `module` *import declaration*: imports a set of translation units (since
  C++20)

### Example

```cpp
export module foo;

import bar;   // imports all module interface units of module bar
import :baz;  // imports the so-named module partition baz of module foo
import <set>; // imports a synthesized header unit formed from header <set>
```

---
*Source: https://en.cppreference.com/w/cpp/keyword/import*
