# Function objects

A *function object* is any object for which the function call operator is
defined. C++ provides many built-in function objects as well as support for
creation and manipulation of new function objects.

### Function invocation

The exposition-only operation `INVOKE(f, arg_0, arg_1, arg_2, ..., arg_N)` is
defined as follows:
Let type `Obj` be the unqualified type of `arg_0` (i.e.,
std::remove_cv<std::remove_reference<decltype(arg_0)>::type>::type)
- If `f` is a pointer to member function of class `C`, then `INVOKE(f, obj,
  arg_1, arg_2, ..., arg_N)` is equivalent to:
- If `std::is_same<C, Obj>::value || std::is_base_of<C, Obj>::value` is `true`
- If `Obj` is a specialization of `std::reference_wrapper`
- Otherwise
- Otherwise, if `N == 0` and `f` is a pointer to data member of class `C`, then
  `INVOKE(mptr, obj)` is equivalent to:
- If `std::is_same<C, Obj>::value || std::is_base_of<C, Obj>::value` is `true`
- If `Obj` is a specialization of `std::reference_wrapper`
- Otherwise
- Otherwise
The exposition-only operation `INVOKE<R>(f, arg_0, arg_1, arg_2, ..., arg_N)` is
defined as follows:
- If `R` is (possibly cv-qualified) void
- Otherwise
Let type `Actual` be decltype(`INVOKE`(f, arg_0, arg_1, arg_2, ..., arg_N))
- If `std::reference_converts_from_temporary_v <R, Actual>` is `true`
*(since C++23)*
*(since C++11)*

Let type `Actual` be decltype(`INVOKE`(f, arg_0, arg_1, arg_2, ..., arg_N))
- If `std::reference_converts_from_temporary_v <R, Actual>` is `true`
*(since C++23)*

`std::invoke` and `std::invoke_r`(since C++23) can invoke any Callable object
with given arguments according to the rules of `INVOKE` and `INVOKE<R>`(since
C++23).

- **invokeinvoke_r (C++17)(C++23)** — invokes any Callable object with given
  arguments and possibility to specify return type(since C++23) (function
  template)

### Function wrappers

`std::function` provides support for storing arbitrary function objects.

- **function (C++11)** — wraps callable object of any copy constructible type
  with specified function call signature (class template)
- **move_only_function (C++23)** — wraps callable object of any type with
  specified function call signature (class template)
- **copyable_function (C++26)** — refinement of `std::move_only_function` that
  wraps callable object of any copy constructible type (class template)
- **function_ref (C++26)** — non-owning reference to any callable object with
  specified function call signature (class template)
- **bad_function_call (C++11)** — the exception thrown when invoking an empty
  `std::function` (class)
- **mem_fn (C++11)** — creates a function object out of a pointer to a member
  (function template)

### Identity

`std::identity` is the identity function object: it returns its argument
unchanged.

- **identity (C++20)** — function object that returns its argument unchanged
  (class)

### Partial function application

`std::bind_front` and `std::bind` provide support for partial function
application, i.e. binding arguments to functions to produce new functions.

- **bind_frontbind_back (C++20)(C++23)** — bind a variable number of arguments,
  in order, to a function object (function template)
- **bind (C++11)** — binds one or more arguments to a function object (function
  template)
- **is_bind_expression (C++11)** — indicates that an object is `std::bind`
  expression or can be used as one (class template)
- **is_placeholder (C++11)** — indicates that an object is a standard
  placeholder or can be used as one (class template)
- **_1, _2, _3, _4, ... (C++11)** — placeholders for the unbound arguments in a
  `std::bind` expression (constant)

### Negators

`std::not_fn` creates a function object that negates the result of the callable
object passed to it.

- **not_fn (C++17)** — creates a function object that returns the complement of
  the result of the function object it holds (function template)

### Searchers

Searchers implementing several string searching algorithms are provided and can
be used either directly or with `std::search`.

- **default_searcher (C++17)** — standard C++ library search algorithm
  implementation (class template)
- **boyer_moore_searcher (C++17)** — Boyer-Moore search algorithm implementation
  (class template)
- **boyer_moore_horspool_searcher (C++17)** — Boyer-Moore-Horspool search
  algorithm implementation (class template)

### Reference wrappers

Reference wrappers allow reference arguments to be stored in copyable function
objects:

- **reference_wrapper (C++11)** — CopyConstructible and CopyAssignable reference
  wrapper (class template)
- **refcref (C++11)(C++11)** — creates a `std::reference_wrapper` with a type
  deduced from its argument (function template)
- **unwrap_referenceunwrap_ref_decay (C++20)(C++20)** — get the reference type
  wrapped in `std::reference_wrapper` (class template)

### Operator function objects

C++ defines several function objects that represent common arithmetic and
logical operations:

**Arithmetic operations**

- **plus** — function object implementing `x + y` (class template)
- **minus** — function object implementing `x - y` (class template)
- **multiplies** — function object implementing `x * y` (class template)
- **divides** — function object implementing `x / y` (class template)
- **modulus** — function object implementing `x % y` (class template)
- **negate** — function object implementing `-x` (class template)

**Comparisons**

- **equal_to** — function object implementing `x == y` (class template)
- **not_equal_to** — function object implementing `x != y` (class template)
- **greater** — function object implementing `x > y` (class template)
- **less** — function object implementing `x < y` (class template)
- **greater_equal** — function object implementing `x >= y` (class template)
- **less_equal** — function object implementing `x <= y` (class template)

**Logical operations**

- **logical_and** — function object implementing `x && y` (class template)
- **logical_or** — function object implementing `x || y` (class template)
- **logical_not** — function object implementing `!x` (class template)

**Bitwise operations**

- **bit_and** — function object implementing `x & y` (class template)
- **bit_or** — function object implementing `x | y` (class template)
- **bit_xor** — function object implementing `x ^ y` (class template)
- **bit_not (C++14)** — function object implementing `~x` (class template)

### Transparent function objects
Associative containers and unordered associative containers(since C++20) provide
heterogenous lookup and erasure(since C++23) operations, but they are only
enabled if the supplied function object type `T` is *transparent* ﻿: the
qualified identifier `T::is_transparent` is valid and denotes a type.
All transparent function object types in the standard library defines a nested
type `is_transparent`. However, user-defined transparent function object types
do not need to directly provide `is_transparent` as a nested type: it can be
defined in a base class, as long as `T::is_transparent` satisfies the
transparent requirement stated above.
C++14 defines several function objects specializations that represent common
arithmetic and logical operations, with parameter and return type deduced:
**Arithmetic operations**
- **plus<void> (C++14)** — function object implementing `x + y` deducing
  argument and return types (class template specialization)
- **minus<void> (C++14)** — function object implementing `x - y` deducing
  argument and return types (class template specialization)
- **multiplies<void> (C++14)** — function object implementing `x * y` deducing
  argument and return types (class template specialization)
- **divides<void> (C++14)** — function object implementing `x / y` deducing
  argument and return types (class template specialization)
- **modulus<void> (C++14)** — function object implementing `x % y` deducing
  argument and return types (class template specialization)
- **negate<void> (C++14)** — function object implementing `-x` deducing argument
  and return types (class template specialization)
**Comparisons**
- **equal_to<void> (C++14)** — function object implementing `x == y` deducing
  argument and return types (class template specialization)
- **not_equal_to<void> (C++14)** — function object implementing `x != y`
  deducing argument and return types (class template specialization)
- **greater<void> (C++14)** — function object implementing `x > y` deducing
  argument and return types (class template specialization)
- **less<void> (C++14)** — function object implementing `x < y` deducing
  argument and return types (class template specialization)
- **greater_equal<void> (C++14)** — function object implementing `x >= y`
  deducing argument and return types (class template specialization)
- **less_equal<void> (C++14)** — function object implementing `x <= y` deducing
  argument and return types (class template specialization)
**Logical operations**
- **logical_and<void> (C++14)** — function object implementing `x && y` deducing
  argument and return types (class template specialization)
- **logical_or<void> (C++14)** — function object implementing `x || y` deducing
  argument and return types (class template specialization)
- **logical_not<void> (C++14)** — function object implementing `!x` deducing
  argument and return types (class template specialization)
**Bitwise operations**
- **bit_and<void> (C++14)** — function object implementing `x & y` deducing
  argument and return types (class template specialization)
- **bit_or<void> (C++14)** — function object implementing `x | y` deducing
  argument and return types (class template specialization)
- **bit_xor<void> (C++14)** — function object implementing `x ^ y` deducing
  argument and return types (class template specialization)
- **bit_not<void> (C++14)** — function object implementing `~x` deducing
  argument and return types (class template specialization)
*(since C++14)*

### Constrained comparison function objects
The following comparison function objects are constrained. The equality
operators (`ranges::equal_to` and `ranges::not_equal_to`) require the types of
the arguments to model `equality_comparable_with`. The relational operators
(`ranges::less`, `ranges::greater`, `ranges::less_equal`, and
`ranges::greater_equal`) require the types of the arguments to model
`totally_ordered_with`. The three-way comparison operator (`compare_three_way`)
requires the type to model `three_way_comparable_with`.
- **ranges::equal_to (C++20)** — function object implementing `x == y` (class)
- **ranges::not_equal_to (C++20)** — function object implementing `x != y`
  (class)
- **ranges::less (C++20)** — function object implementing `x < y` (class)
- **ranges::greater (C++20)** — function object implementing `x > y` (class)
- **ranges::less_equal (C++20)** — function object implementing `x <= y` (class)
- **ranges::greater_equal (C++20)** — function object implementing `x >= y`
  (class)
- **compare_three_way (C++20)** — function object implementing `x <=> y` (class)
*(since C++20)*

### Old binders and adaptors
Several utilities that provided early functional support are deprecated in C++11
and removed in C++17 (old negators are deprecated in C++17 and removed in
C++20):
**Base**
- **unary_function (deprecated in C++11)(removed in C++17)** —
  adaptor-compatible unary function base class (class template)
- **binary_function (deprecated in C++11)(removed in C++17)** —
  adaptor-compatible binary function base class (class template)
**Binders**
- **binder1stbinder2nd (deprecated in C++11)(removed in C++17)** — function
  object holding a binary function and one of its arguments (class template)
- **bind1stbind2nd (deprecated in C++11)(removed in C++17)** — binds one
  argument to a binary function (function template)
**Function adaptors**
- **pointer_to_unary_function (deprecated in C++11)(removed in C++17)** —
  adaptor-compatible wrapper for a pointer to unary function (class template)
- **pointer_to_binary_function (deprecated in C++11)(removed in C++17)** —
  adaptor-compatible wrapper for a pointer to binary function (class template)
- **ptr_fun (deprecated in C++11)(removed in C++17)** — creates an
  adaptor-compatible function object wrapper from a pointer to function
  (function template)
- **mem_fun_tmem_fun1_tconst_mem_fun_tconst_mem_fun1_t (deprecated in
  C++11)(removed in C++17)** — wrapper for a pointer to nullary or unary member
  function, callable with a pointer to object (class template)
- **mem_fun (deprecated in C++11)(removed in C++17)** — creates a wrapper from a
  pointer to member function, callable with a pointer to object (function
  template)
- **mem_fun_ref_tmem_fun1_ref_tconst_mem_fun_ref_tconst_mem_fun1_ref_t
  (deprecated in C++11)(removed in C++17)** — wrapper for a pointer to nullary
  or unary member function, callable with a reference to object (class template)
- **mem_fun_ref (deprecated in C++11)(removed in C++17)** — creates a wrapper
  from a pointer to member function, callable with a reference to object
  (function template)
- **unary_negate (deprecated in C++17)(removed in C++20)** — wrapper function
  object returning the complement of the unary predicate it holds (class
  template)
- **binary_negate (deprecated in C++17)(removed in C++20)** — wrapper function
  object returning the complement of the binary predicate it holds (class
  template)
- **not1 (deprecated in C++17)(removed in C++20)** — constructs custom
  `std::unary_negate` object (function template)
- **not2 (deprecated in C++17)(removed in C++20)** — constructs custom
  `std::binary_negate` object (function template)
*(until C++20)*

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 185 | C++98 | using function objects improved the program efficiency |
      removed the claim
  LWG 660 | C++98 | function objects for bitwise operations are missing | added
  LWG 2219 | C++11 | `INVOKE` did not handle `std::reference_wrapper` correctly
      | handles correctly
  LWG 2420 | C++11 | `INVOKE<R>` did not discard the return value if `R` is void
      | discards the return value in this case
  LWG 2926 (P0604R0) | C++11 | the syntax of the `INVOKE` operation with a
      return type `R` was `INVOKE(f, t1, t2, ..., tN, R)` | changed to
      `INVOKE<R>(f, t1, t2, ..., tN)`
  LWG 3655 | C++11 | `INVOKE` did not handle unions correctly due to the
      resolution of LWG issue 2219 | handles correctly

---
*Source: https://en.cppreference.com/w/cpp/utility/functional*
