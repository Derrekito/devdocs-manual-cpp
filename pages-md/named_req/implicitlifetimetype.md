# C++ named requirements: ImplicitLifetimeType

Note: the standard doesn't define a named requirement with this name. This is a
type category defined by the core language. It is included here as a named
requirement only for consistency.

### Requirements

The following types are collectively called *implicit-lifetime types*:

- scalar types
- implicit-lifetime class types
- array types
- cv-qualified versions of these types

### Notes

Certain operations may implicitly create and start the lifetime of objects of
implicit-lifetime types, if doing so would prevent undefined behavior. However,
such operations do not start the lifetimes of subobjects of such objects that
are not themselves of implicit-lifetime types.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 2489 | C++20 | an operation that begins the lifetime of a char array
      implicitly creates objects | it does not

---
*Source: https://en.cppreference.com/w/cpp/named_req/ImplicitLifetimeType*
