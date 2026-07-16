# C++ named requirements: EqualityComparable

The type must work with == operator and the result should have standard
semantics.

### Requirements

The type `T` satisfies EqualityComparable if

Given

- `a`, `b` and `c`, expressions of type `T` or(since C++11) `const T`.

The following expressions must be valid and have their specified effects:

  Expression | Return type | Requirements
  `a == b` | implicitly convertible to `bool` | Establishes an equivalence
      relation, that is, it satisfies the following properties: For all values
      of `a`, `a == a` yields `true`. If `a == b`, then `b == a`. If `a == b`
      and `b == c`, then `a == c`.

### Notes

To satisfy this requirement, types that do not have built-in comparison
operators have to provide a user-defined operator==.

For the types that are both EqualityComparable and LessThanComparable, the C++
standard library makes a distinction between *equality*, which is the value of
the expression `a == b` and *equivalence*, which is the value of the expression
`!(a < b) && !(b < a)`.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 283 | C++98 | even if `T` is EqualityComparable, the requirements did not
      apply to `const T` objects | they apply to `const T` instead of `T`

### See also

- **equality_comparableequality_comparable_with (C++20)** — specifies that
  operator `==` is an equivalence relation (concept)

---
*Source: https://en.cppreference.com/w/cpp/named_req/EqualityComparable*
