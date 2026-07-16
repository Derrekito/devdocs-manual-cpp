# C++ named requirements: NumericType

Specifies that the type can be used as the template argument of `std::valarray`.

### Requirements

For a type `T` to be a NumericType, it must be a cv-unqualified object type
that:

- Meets the DefaultConstructible, CopyConstructible, CopyAssignable and
  Destructible requirements.
- No operation on `T` may throw exceptions.

---
*Source: https://en.cppreference.com/w/cpp/named_req/NumericType*
