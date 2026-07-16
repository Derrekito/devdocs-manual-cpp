# C++ named requirements: MoveAssignable (since C++11)

Specifies that an instance of the type can be assigned from an rvalue argument.

### Requirements

The type `T` satisfies MoveAssignable if

Given

- `t`, a modifiable lvalue expression of type `T`,
- `rv`, an rvalue expression of type `T`.

The following expressions must be valid and have their specified effects.

  Expression | Return type | Return value | Post-conditions
  `t = rv` | `T&` | `t` | If `t` and `rv` do not refer to the same object, the
      value of `t` is equivalent to the value of `rv` before the assignment. The
      new value of `rv` is unspecified.

### Notes

The type does not have to implement move assignment operator in order to satisfy
this type requirement: a copy assignment operator that takes its parameter by
value or as a `const Type&`, will bind to rvalue argument.

If a MoveAssignable class implements a move assignment operator, it may also
implement move semantics to take advantage of the fact that the value of `rv`
after assignment is unspecified.

### See also

- **is_move_assignableis_trivially_move_assignableis_nothrow_move_assignable
  (C++11)(C++11)(C++11)** — checks if a type has a move assignment operator
  (class template)
- **assignable_from (C++20)** — specifies that a type is assignable from another
  type (concept)

---
*Source: https://en.cppreference.com/w/cpp/named_req/MoveAssignable*
