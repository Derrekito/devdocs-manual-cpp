# Default-initialization

This is the initialization performed when an object is constructed with no
initializer.

### Syntax

```text
T object ﻿;    (1)
new T    (2)
```

### Explanation

Default-initialization is performed in three situations:

1) when a variable with automatic, static, or thread-local storage duration is
   declared with no initializer;

2) when an object with dynamic storage duration is created by a new-expression
   with no initializer;

3) when a base class or a non-static data member is not mentioned in a
   constructor initializer list and that constructor is called.

The effects of default-initialization are:

- if `T` is a (possibly cv-qualified) non-POD(until C++11) class type, the
  constructors are considered and subjected to overload resolution against the
  empty argument list. The constructor selected (which is one of the default
  constructors) is called to provide the initial value for the new object;
- if `T` is an array type, every element of the array is default-initialized;
- otherwise, no initialization is performed (see notes).

Only (possibly cv-qualified) non-POD class types (or arrays thereof) with
automatic storage duration were considered to be default-initialized when no
initializer is used. Scalars and POD types with dynamic storage duration were
considered to be not initialized (since C++11, this situation was reclassified
as a form of default-initialization).
*(until C++11)*

- each direct non-static data member `M` of `T` is of class type `X` (or array
  thereof), `X` is const-default-constructible, and
- `T` has no direct variant members, and
*(until C++11)*

- each direct non-variant non-static data member `M` of `T` has a default member
  initializer or, if `M` is of class type `X` (or array thereof), `X` is
  const-default-constructible,
- if `T` is a union with at least one non-static data member, exactly one
  variant member has a default member initializer,
- if `T` is not a union, for each anonymous union member with at least one
  non-static data member (if any), exactly one non-static data member has a
  default member initializer, and
*(since C++11)*

---
*Source: https://en.cppreference.com/w/cpp/language/default_initialization*
