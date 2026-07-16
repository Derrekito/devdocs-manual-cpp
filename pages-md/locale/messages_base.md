# std::messages_base

```cpp
class messages_base;
```

The class `std::messages_base` provides a type definition which is inherited and
used by the `std::messages` facets.

### Member types

- **`catalog`** — An unspecified signed integer type

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2028 | C++98 | `catalog` was defined as int | defined as an unspecified
      signed integer type[1]

1. The catalog type `nl_catd` used in POSIX messaging API is a typedef of an
   unspecified type.

### See also

- **messages** — implements retrieval of strings from message catalogs (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/locale/messages_base*
