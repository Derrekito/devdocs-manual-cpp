# C++ named requirements: NullablePointer (since C++11)

Specifies that the type is a pointer-like object which can be compared to
`std::nullptr_t` objects.

### Requirements

The type must meet all of the following requirements:

- EqualityComparable
- DefaultConstructible
- CopyConstructible
- CopyAssignable

- lvalues of the type are Swappable
*(until C++23)*

- Swappable
*(since C++23)*

- Destructible

In addition, a value-initialized object of the type must produce a null value of
that type. This null value shall only be equivalent to itself. Default
initialization of the type may have an indeterminate value.

An object of the type must be contextually convertible to `bool`. The effect of
this conversion returns `false` if the value is equivalent to its null value and
`true` otherwise.

None of the operations that this type performs may throw exceptions.

The type must satisfy the following additional expressions, given two values `p`
and `q` that are of the type, and that `np` is a value of `std::nullptr_t` type
(possibly `const` qualified):

- **`Type p(np);` `Type p = np;`** — Afterwards, `p` is equivalent to `nullptr`
- **`Type(np)`** — A temporary object that is equivalent to `nullptr`
- **`p = np`** — Must return a `Type&`, and afterwards, `p` is equivalent to
  `nullptr`
- **`p != q`** — Must return a value that is contextually convertible to `bool`
  (until C++23) `decltype(p != q)` models `boolean-testable` (since C++23) The
  effect is `!(p == q)`
- **Must return a value that is contextually convertible to `bool`** — (until
  C++23)
- **`decltype(p != q)` models `boolean-testable`** — (since C++23)
- **`p == np` `np == p`** — Must return a value that is contextually convertible
  to `bool` (until C++23) `decltype(p == np)` and `decltype(np == p)` each model
  `boolean-testable` (since C++23) The effect is `(p == Type())`
- **Must return a value that is contextually convertible to `bool`** — (until
  C++23)
- **`decltype(p == np)` and `decltype(np == p)` each model `boolean-testable`**
  — (since C++23)
- **`p != np` `np != p`** — Must return a value that is contextually convertible
  to `bool` (until C++23) `decltype(p != np)` and `decltype(np != p)` each model
  `boolean-testable` (since C++23) The effect is `!(p == np)`
- **Must return a value that is contextually convertible to `bool`** — (until
  C++23)
- **`decltype(p != np)` and `decltype(np != p)` each model `boolean-testable`**
  — (since C++23)

### Notes

Note that dereferencing (`operator*` or `operator->`) is not required of a
NullablePointer type. A minimalistic type that satisfies these requirements is

```cpp
class handle
{
    int id = 0;
public:
    handle() = default;
    handle(std::nullptr_t) {}
    explicit operator bool() const { return id != 0; }
    friend bool operator==(handle l, handle r) { return l.id == r.id; }
    friend bool operator!=(handle l, handle r) { return !(l == r); }
    // or only a defaulted operator== (since C++20)
};
```

### Standard library

The following types must satisfy NullablePointer:

- The member types `X::pointer`, `X::const_pointer`, `X::void_pointer` and
  `X::const_void_pointer` of every Allocator type `X`.
- The member type `X::pointer` of `std::unique_ptr`.
- The type `std::exception_ptr`.

---
*Source: https://en.cppreference.com/w/cpp/named_req/NullablePointer*
