# C++ named requirements: DefaultConstructible

Specifies that an instance of the type can be default constructed.

### Requirements

The type `T` satisfies DefaultConstructible if

Given

- `u`, an arbitrary identifier.

The following expressions must be valid and have their specified effects.

  Expression | Post-conditions
  `T u;` | The object `u` is default-initialized.
  `T u{};` | The object `u` is value-initialized or aggregate-initialized.
  `T()` `T{}` | A temporary object of type `T` is value-initialized or
      aggregate-initialized.

### Notes

For objects of non-aggregate class type, a public default constructor must be
defined (either user-defined or implicitly defined) to satisfy
DefaultConstructible.

Non-const objects of non-class object type are always DefaultConstructible.

Const non-class types are not DefaultConstructible.

Const aggregate types are not DefaultConstructible if any of their members is an
object of non-class type.

Non-object types (function types, reference types, and the (possibly
cv-qualified) type void) as well as the const non-object types are never
DefaultConstructible.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 724 | C++98 | the requirements of DefaultConstructible were missing |
      added

### See also

-
  **is_default_constructibleis_trivially_default_constructibleis_nothrow_default_constructible
  (C++11)(C++11)(C++11)** — checks if a type has a default constructor (class
  template)
- **default_initializable (C++20)** — specifies that an object of a type can be
  default constructed (concept)

---
*Source: https://en.cppreference.com/w/cpp/named_req/DefaultConstructible*
