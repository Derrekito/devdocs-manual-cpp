# std::assignable_from

```cpp
template< class LHS, class RHS >
concept assignable_from =
    std::is_lvalue_reference_v<LHS> &&
    std::common_reference_with<
        const std::remove_reference_t<LHS>&,
        const std::remove_reference_t<RHS>&> &&
    requires(LHS lhs, RHS&& rhs) {
        { lhs = std::forward<RHS>(rhs) } -> std::same_as<LHS>;
    };  // (since C++20)
```

The concept `assignable_from<LHS, RHS>` specifies that an expression of the type
and value category specified by `RHS` can be assigned to an lvalue expression
whose type is specified by `LHS`.

### Semantic requirements

Given

- `lhs`, an lvalue that refers to an object `lcopy` such that `decltype((lhs))`
  is `LHS`,
- `rhs`, an expression such that `decltype((rhs))` is `RHS`,
- `rcopy`, a distinct object that is equal to `rhs`,

`assignable_from<LHS, RHS>` is modeled only if

- `std::addressof(lhs = rhs) == std::addressof(lcopy)` (i.e., the assignment
  expression yields an lvalue referring to the left operand);
- After evaluating `lhs = rhs`: `lhs` is equal to `rcopy`, unless `rhs` is a
  non-const xvalue that refers to `lcopy` (i.e., the assignment is a
  self-move-assignment), if `rhs` is a glvalue: If it is a non-const xvalue, the
  object to which it refers is in a valid but unspecified state; Otherwise, the
  object it refers to is not modified;

### Equality preservation

Expressions declared in requires expressions of the standard library concepts
are required to be equality-preserving (except where stated otherwise).

### Notes

Assignment need not be a total function. In particular, if assigning to some
object `x` can cause some other object `y` to be modified, then `x = y` is
likely not in the domain of `=`. This typically happens if the right operand is
owned directly or indirectly by the left operand (e.g., with smart pointers to
nodes in a node-based data structure, or with something like
`std::vector<std::any>`).

### See also

- **is_assignableis_trivially_assignableis_nothrow_assignable
  (C++11)(C++11)(C++11)** — checks if a type has an assignment operator for a
  specific argument (class template)

---
*Source: https://en.cppreference.com/w/cpp/concepts/assignable_from*
