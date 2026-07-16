# Constant expressions

Defines an expression that can be evaluated at compile time.

Such expressions can be used as non-type template arguments, array sizes, and in
other contexts that require constant expressions, e.g.

```cpp
int n = 1;
std::array<int, n> a1;  // error: n is not a constant expression
const int cn = 2;
std::array<int, cn> a2; // OK: cn is a constant expression
```

### Core constant expressions

A *core constant expression* is any expression whose evaluation **would not**
evaluate any one of the following:

1. the `this` pointer, except in a constexpr function that is being evaluated as
   part of the expression
1. a control flow that passes through a declaration of a variable with static or
   thread-local storage duration, and unusable in constant expressions
1. a function call expression that calls a function (or a constructor) that is
   not declared constexpr constexpr int n = std::numeric_limits<int>::max(); //
   OK: max() is constexpr constexpr int m = std::time(nullptr); // Error:
   std::time() is not constexpr
1. a function call to a constexpr function which is declared, but not defined
1. a function call to a constexpr function/constructor template instantiation
   where the instantiation fails to satisfy constexpr function/constructor
   requirements.
1. a function call to a constexpr virtual function, invoked on an object not
   usable in constant expressions and whose lifetime began outside this
   expression.
1. an expression that would exceed the implementation-defined limits
1. an expression whose evaluation leads to any form of core language undefined
   behavior (including signed integer overflow, division by zero, pointer
   arithmetic outside array bounds, etc). Whether standard library undefined
   behavior is detected is unspecified. constexpr double d1 = 2.0 / 1.0; // OK
   constexpr double d2 = 2.0 / 0.0; // Error: not defined constexpr int n =
   std::numeric_limits<int>::max() + 1; // Error: overflow int x, y, z[30];
   constexpr auto e1 = &y - &x; // Error: undefined constexpr auto e2 = &z[20] -
   &z[3]; // OK constexpr std::bitset<2> a; constexpr bool b = a[2]; // UB, but
   unspecified if detected
1. (until C++17) a lambda expression
1. an lvalue-to-rvalue implicit conversion unless applied to a non-volatile
   literal-type glvalue that ... designates an object that is usable in constant
   expressions, int main() { const std::size_t tabsize = 50; int tab[tabsize];
   // OK: tabsize is a constant expression // because tabsize is usable in
   constant expressions // because it has const-qualified integral type, and //
   its initializer is a constant initializer std::size_t n = 50; const
   std::size_t sz = n; int tab2[sz]; // error: sz is not a constant expression
   // because sz is not usable in constant expressions // because its
   initializer was not a constant initializer } refers to a non-volatile object
   whose lifetime began within the evaluation of this expression
1. an lvalue-to-rvalue implicit conversion or modification applied to a
   non-active member of a union or its subobject (even if it shares a common
   initial sequence with the active member)
1. an lvalue-to-rvalue implicit conversion on an object whose value is
   indeterminate
1. an invocation of implicit copy/move constructor/assignment for a union whose
   active member is mutable (if any), with lifetime beginning outside the
   evaluation of this expression
1. (until C++20) an assignment expression that would change the active member of
   a union
1. an id-expression referring to a variable or a data member of reference type,
   unless the reference is usable in constant expressions or its lifetime began
   within the evaluation of this expression
1. conversion from pointer to void to any pointer-to-object type(until C++26) to
   a pointer-to-object type `T*` unless the pointer points to an object whose
   type is similar to `T`(since C++26)
1. (until C++20) `dynamic_cast`
1. `reinterpret_cast`
1. (until C++20) pseudo-destructor call
1. (until C++14) an increment or a decrement operator
1. (since C++14) modification of an object, unless the object has non-volatile
   literal type and its lifetime began within the evaluation of the expression
   constexpr int incr(int& n) { return ++n; } constexpr int g(int k) { constexpr
   int x = incr(k); // error: incr(k) is not a core constant // expression
   because lifetime of k // began outside the expression incr(k) return x; }
   constexpr int h(int k) { int x = incr(k); // OK: x is not required to be
   initialized // with a core constant expression return x; } constexpr int y =
   h(1); // OK: initializes y with the value 2 // h(1) is a core constant
   expression because // the lifetime of k begins inside the expression h(1)
1. (since C++20) a destructor call or pseudo destructor call for an object whose
   lifetime did not begin within the evaluation of this expression
1. (until C++20) a `typeid` expression applied to a glvalue of polymorphic type
1. a new-expression or a call to `std::allocator<T>::allocate`, unless the
   selected allocation function is a replaceable global allocation function and
   the allocated storage is deallocated within the evaluation of this
   expression(since C++20)
1. a delete-expression or a call to `std::allocator<T>::deallocate`, unless it
   deallocates a region of storage allocated within the evaluation of this
   expression(since C++20)
1. (since C++20) Coroutines: an await-expression or a yield-expression
1. (since C++20) a three-way comparison when the result is unspecified
1. an equality or relational operator whose result is unspecified
1. (until C++14) an assignment or a compound assignment operator
1. a throw expression
1. an asm-declaration
1. an invocation of the `va_arg` macro, whether an invocation of the `va_start`
   macro can be evaluated is unspecified
1. a `goto` statement
1. a `dynamic_cast` or `typeid` expression that would throw an exception
1. inside a lambda-expression, a reference to `this` or to a variable defined
   outside that lambda, if that reference would be an odr-use void g() { const
   int n = 0; constexpr int j = *&n; // OK: outside of a lambda-expression [=] {
   constexpr int i = n; // OK: 'n' is not odr-used and not captured here.
   constexpr int j = *&n; // Ill-formed: '&n' would be an odr-use of 'n'. }; }
   note that if the ODR-use takes place in a function call to a closure, it does
   not refer to `this` or to an enclosing variable, since it accesses a
   closure's data member instead // OK: 'v' & 'm' are odr-used but do not occur
   in a constant-expression // within the nested lambda auto monad = [](auto v){
   return [=]{ return v; }; }; auto bind = [](auto m){ return [=](auto fvm){
   return fvm(m()); }; }; // OK to have captures to automatic objects created
   during constant expression evaluation. static_assert(bind(monad(2))(monad)()
   == monad(2)()); (since C++17)

Note: Just being a core constant expression does not have any direct semantic
meaning: an expression has to be one of the subsets of constant expressions (see
below) to be used in certain contexts.

### Constant expression

A *constant expression* is either

- an lvalue(until C++14)a glvalue(since C++14) core constant expression that
  refers to

- an object with static storage duration that is a temporary, but whose value
  satisfies the constraints for prvalues below, or
*(since C++14)*

- a non-immediate(since C++20) function
- a prvalue core constant expression whose value satisfies the following
  constraints:
- if the value is an object of class type, each non-static data member of
  reference type refers to an entity that satisfies the constraints for
  lvalues(until C++14)glvalues(since C++14) above
- if the value is of pointer type, it holds

- if the value is of pointer-to-member-function type, it does not designate an
  immediate function
*(since C++20)*

- if the value is an object of class or array type, each subobject satisfies
  these constraints for values

```cpp
void test()
{
    static const int a = std::random_device{}();
    constexpr const int& ra = a; // OK: a is a glvalue constant expression
    constexpr int ia = a; // Error: a is not a prvalue constant expression

    const int b = 42;
    constexpr const int& rb = b; // Error: b is not a glvalue constant expression
    constexpr int ib = b; // OK: b is a prvalue constant expression
}
```

#### Integral constant expression

*Integral constant expression* is an expression of integral or unscoped
enumeration type implicitly converted to a prvalue, where the converted
expression is a core constant expression. If an expression of class type is used
where an integral constant expression is expected, the expression is
contextually implicitly converted to an integral or unscoped enumeration type.

The following contexts require an integral constant expression:

- array bounds
- the dimensions in new-expressions other than the first
*(until C++14)*

- bit-field lengths
- enumeration initializers when the underlying type is not fixed
- alignments.

#### Converted constant expression

A *converted constant expression* of type `T` is an expression implicitly
converted to type T, where the converted expression is a constant expression,
and the implicit conversion sequence contains only:

- constexpr user-defined conversions (so a class can be used where integral type
  is expected)
- lvalue-to-rvalue conversions
- integral promotions
- non-narrowing integral conversions

- array-to-pointer conversions
- function-to-pointer conversions
- function pointer conversions (pointer to noexcept function to pointer to
  function)
- qualification conversions
- null pointer conversions from `std::nullptr_t`
- null member pointer conversions from `std::nullptr_t`
*(since C++17)*

- And if any reference binding takes place, it is direct binding (not one that
  constructs a temporary object)

The following contexts require a converted constant expression:

- case expressions
- enumerator initializers when the underlying type is fixed

- array bounds
- the dimensions in new-expressions other than the first
*(since C++14)*

- integral and enumeration (until C++17)non-type template arguments.

- the index of pack indexing expression and pack indexing specifier.
*(since C++26)*

A *contextually converted constant expression of type bool* is an expression,
contextually converted to bool, where the converted expression is a constant
expression and the conversion sequence contains only the conversions above.

The following contexts require a contextually converted constant expression of
type bool:

- noexcept specifications

- static_assert declarations
*(until C++23)*

- constexpr if-statements
*(since C++17)(until C++23)*

- conditional explicit specifiers
*(since C++20)*

#### Historical categories

Categories of constant expressions listed below are no longer used in the
standard since C++14:

- A *literal constant expression* is a prvalue core constant expression of
  non-pointer literal type (after conversions as required by context). A literal
  constant expression of array or class type requires that each subobject is
  initialized with a constant expression.
- A *reference constant expression* is an lvalue core constant expression that
  designates an object with static storage duration or a function.
- An *address constant expression* is a prvalue core constant expression (after
  conversions required by context) of type `std::nullptr_t` or of a pointer
  type, which points to an object with static storage duration, one past the end
  of an array with static storage duration, to a function, or is a null pointer.

### Constant subexpression
A *constant subexpression* is an expression whose evaluation as subexpression of
an expression `e` would not prevent `e` from being a core constant expression,
where `e` is not any of the following expressions:
- `throw` expression
- yield expression
*(since C++20)*
- assignment expression
- comma expression
*(since C++17)*

- yield expression
*(since C++20)*

### Usable in constant expressions

In the list above, a variable is *usable in constant expressions* at a point `P`
if

- the variable is
- a constexpr variable, or
- it is a constant-initialized variable
- and the definition of the variable is reachable from `P`

- and, if `P` is not in the same translation unit as the definition of the
  variable (that is, the definition is imported), the variable is not
  initialized to point to, or refer to, or have a (possibly recursive) subobject
  that points to or refers to, a translation-unit-local entity that is usable in
  constant expressions
*(since C++20)*

An object or reference is *usable in constant expressions* if it is

- a variable that is usable in constant expressions, or

- a template parameter object, or
*(since C++20)*

- a string literal object, or
- a non-mutable subobject or reference member of any of the above, or
- a temporary object of non-volatile const-qualified literal type whose lifetime
  is extended to that of a variable that is usable in constant expressions.

```cpp
const std::size_t sz = 10; // sz is usable in constant expressions
```

### Manifestly constant-evaluated expressions

The following expressions (including conversions to the destination type) are
*manifestly constant-evaluated*:

- array bounds
- the dimensions in new-expressions other than the first
- bit-field lengths
- enumeration initializers
- alignments
- case expressions
- non-type template arguments
- expressions in noexcept specifications
- expressions in static_assert declarations
- initializers of constexpr variables

- conditions of constexpr if-statements
*(since C++17)*

- expressions in conditional explicit specifiers
- immediate invocations
- constraint expressions in concept definitions, nested requirements, and
  requires clauses, when determining whether the constraints are satisfied
*(since C++20)*

- initializers of variables with reference type or const-qualified integral or
  enumeration type, but only if the initializers are constant expressions
- initializers of static and thread local variables, but only if all
  subexpressions of the initializers (including constructor calls and implicit
  conversions) are constant expressions (that is, if the initializers are
  constant initializers)

Whether an evaluation occurs in a manifestly constant-evaluated context can be
detected by `std::is_constant_evaluated` and `if consteval`(since C++23).
To test the last two conditions, compilers may first perform a trial constant
evaluation of the initializers. It is not recommended to depend on the result in
this case.
```cpp
int y = 0;
const int a = std::is_constant_evaluated() ? y : 1;
// Trial constant evaluation fails. The constant evaluation is discarded.
// Variable a is dynamically initialized with 1
const int b = std::is_constant_evaluated() ? 2 : y;
// Constant evaluation with std::is_constant_evaluation() == true succeeds.
// Variable b is statically initialized with 2
```
*(since C++20)*

### Functions and variables needed for constant evaluation

Following expressions or conversions are *potentially constant evaluated*:

- manifestly constant-evaluated expressions
- potentially-evaluated expressions
- immediate subexpressions of a braced-init-list (constant evaluation may be
  necessary to determine whether a conversion is narrowing)
- address-of (unary `&`) expressions that occur within a templated entity
  (constant evaluation may be necessary to determine whether such an expression
  is value-dependent)
- subexpressions of one of the above that are not a subexpression of a nested
  unevaluated operand

A function is *needed for constant evaluation* if it is a constexpr function and
named by an expression that is potentially constant evaluated.

A variable is *needed for constant evaluation* if it is either a constexpr
variable or is of non-volatile const-qualified integral type or of reference
type and the id-expression that denotes it is potentially constant evaluated.

Definition of a defaulted function and instantiation of a function template
specialization or variable template specialization(since C++14) are triggered if
the function or variable(since C++14) is needed for constant evaluation.

### Notes

Implementations are not permitted to declare library functions as constexpr
unless the standard says the function is constexpr. Named return value
optimization (NRVO) is not permitted in constant expressions, while return value
optimization (RVO) is mandatory.

  Feature-test macro | Value | Std | Feature
  `__cpp_constexpr_in_decltype` | 201711L | (C++11) (DR) | Generation of
      function and variable definitions when needed for constant evaluation
  `__cpp_constexpr_dynamic_alloc` | 201907L | (C++20) | Operations for dynamic
      storage duration in constexpr functions
  `__cpp_constexpr` | 202306L | (C++26) | Constexpr cast from void*: towards
      constexpr type-erasure

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 1293 | C++11 | it was unspecified whether string literals are usable in
      constant expressions | they are usable
  CWG 1311 | C++11 | volatile glvalues could be used in constant expressions |
      prohibited
  CWG 1312 | C++11 | reinterpret_cast is prohibited in constant expressions, but
      casting to and from void* could achieve the same effect | prohibited
      conversions from type *cv* void* to a pointer-to-object type
  CWG 1313 | C++11 | undefined behavior was permitted; all pointer subtraction
      was prohibited | UB prohibited; same-array pointer subtraction OK
  CWG 1405 | C++11 | for objects that are usable in constant expressions, their
      mutable subobjects were also usable | they are not usable
  CWG 1454 | C++11 | passing constants through constexpr functions via
      references was not allowed | allowed
  CWG 1455 | C++11 | converted constant expressions could only be prvalues | can
      be lvalues
  CWG 1456 | C++11 | an address constant expression could not designate the
      address one past the end of an array | allowed
  CWG 1535 | C++11 | a typeid expression whose operand is of a polymorphic class
      type was not a core constant expression even if no runtime check is
      involved | the operand constraint is limited to glvalues of polymorphic
      class types
  CWG 1581 | C++11 | functions needed for constant evaluation were not required
      to be defined or instantiated | required
  CWG 1694 | C++11 | binding the value of a temporary to a static storage
      duration reference was a constant expression | it is not a constant
      expression
  CWG 1952 | C++11 | standard library undefined behaviors were required to be
      diagnosed | unspecified whether they are diagnosed
  CWG 2126 | C++11 | constant initialized lifetime-extended temporaries of
      const- qualified literal types were not usable in constant expressions |
      usable
  CWG 2167 | C++11 | non-member references local to an evaluation made the
      evaluation non-constexpr | non-member references allowed
  CWG 2299 | C++14 | it was unclear whether macros in `<cstdarg>` can be used in
      constant evaluation | `va_arg` forbidden, `va_start` unspecified
  CWG 2400 | C++11 | invoking a constexpr virtual function on an object not
      usable in constant expressions and whose lifetime began outside the
      expression containing the invocation could be a constant expression | it
      is not a constant expression
  CWG 2418 | C++11 | it was unspecified which object or reference that are not
      variables are usable in constant expressions | specified
  CWG 2490 | C++20 | (pseudo) destructor calls lacked restrictions in constant
      evaluation | restriction added

### See also

- **`constexpr` specifier(C++11)** — specifies that the value of a variable or
  function can be computed at compile time
- **is_literal_type (C++11)(deprecated in C++17)(removed in C++20)** — checks if
  a type is a literal type (class template)

**C documentation for Constant expressions**

---
*Source: https://en.cppreference.com/w/cpp/language/constant_expression*
