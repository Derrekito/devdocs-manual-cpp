# C++ named requirements: CopyAssignable

Specifies that an instance of the type can be copy-assigned from an lvalue
expression.

### Requirements

The type `T` satisfies CopyAssignable if

- The type `T` satisfies MoveAssignable, and

Given

- `t`, a modifiable lvalue expression of type `T`,
- `v`, an lvalue expression of type `T` or `const T` or an rvalue expression of
  type `const T`.

The following expressions must be valid and have their specified effects.

  Expression | Return type | Return value | Post-conditions
  `t = v` | `T&` | `t` | The value of `t` is equivalent to the value of `v`. The
      value of `v` is unchanged.

### See also

- **is_copy_assignableis_trivially_copy_assignableis_nothrow_copy_assignable
  (C++11)(C++11)(C++11)** — checks if a type has a copy assignment operator
  (class template)
- **assignable_from (C++20)** — specifies that a type is assignable from another
  type (concept)

---
*Source: https://en.cppreference.com/w/cpp/named_req/CopyAssignable*
