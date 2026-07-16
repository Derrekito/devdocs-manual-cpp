# std::nullopt_t

```cpp
struct nullopt_t;  // (since C++17)
```

`std::nullopt_t` is an empty class type used to indicate `optional` type with
uninitialized state. In particular, `std::optional` has a constructor with
`nullopt_t` as a single argument, which creates an optional that does not
contain a value.

`std::nullopt_t` must be a non-aggregate LiteralType and cannot have a default
constructor or an initializer-list constructor.

It must have a `constexpr` constructor that takes some implementation-defined
literal type.

### Notes

The constraints on `nullopt_t`'s constructors exist to support both `op = {};`
and `op = nullopt;` as the syntax for disengaging an optional object.

A possible implementation of this class is

```cpp
struct nullopt_t {
    constexpr explicit nullopt_t(int) {}
};
```

### See also

- **nullopt (C++17)** — an object of type `nullopt_t` (constant)

---
*Source: https://en.cppreference.com/w/cpp/utility/optional/nullopt_t*
