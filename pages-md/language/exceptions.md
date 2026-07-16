# Exceptions

Exception handling provides a way of transferring control and information from
some point in the execution of a program to a handler associated with a point
previously passed by the execution (in other words, exception handling transfers
control up the call stack).

An exception can be thrown by a throw-expression, `dynamic_cast`, `typeid`,
new-expression, allocation function, and any of the standard library functions
that are specified to throw exceptions to signal certain error conditions (e.g.
`std::vector::at`, `std::string::substr`, etc).

In order for an exception to be caught, the throw-expression has to be inside a
try-block or inside a function called from a try-block, and there has to be a
catch clause that matches the type of the exception object.

When declaring a function, the following specification(s) may be provided to
limit the types of the exceptions a function may throw:

- dynamic exception specifications
*(until C++17)*

- noexcept specifications
*(since C++11)*

Errors that arise during exception handling are handled by `std::terminate` and
`std::unexpected`(until C++17).

### Usage

While throw-expression can be used to transfer control to an arbitrary block of
code up the execution stack, for arbitrary reasons (similar to `std::longjmp`),
its intended usage is error handling.

#### Error handling

Throwing an exception is used to signal errors from functions, where "errors"
are typically limited to only the following[1][2][3]:

1. Failures to meet the postconditions, such as failing to produce a valid
   return value object.
1. Failures to meet the preconditions of another function that must be called.
1. (for non-private member functions) Failures to (re)establish a class
   invariant.

In particular, this implies that the failures of constructors (see also RAII)
and most operators should be reported by throwing exceptions.

In addition, so-called *wide contract* functions use exceptions to indicate
unacceptable inputs, for example, `std::string::at` has no preconditions, but
throws an exception to indicate index out of range.

#### Exception safety

After the error condition is reported by a function, additional guarantees may
be provided with regards to the state of the program. The following four levels
of exception guarantee are generally recognized[4][5][6], which are strict
supersets of each other:

1. *Nothrow (or nofail) exception guarantee* — the function never throws
   exceptions. Nothrow (errors are reported by other means or concealed) is
   expected of destructors and other functions that may be called during stack
   unwinding. The destructors are `noexcept` by default.(since C++11) Nofail
   (the function always succeeds) is expected of swaps, move constructors, and
   other functions used by those that provide strong exception guarantee.
1. *Strong exception guarantee* — If the function throws an exception, the state
   of the program is rolled back to the state just before the function call (for
   example, `std::vector::push_back`).
1. *Basic exception guarantee* — If the function throws an exception, the
   program is in a valid state. No resources are leaked, and all objects'
   invariants are intact.
1. *No exception guarantee* — If the function throws an exception, the program
   may not be in a valid state: resource leaks, memory corruption, or other
   invariant-destroying errors may have occurred.

Generic components may, in addition, offer *exception-neutral guarantee*: if an
exception is thrown from a template parameter (e.g. from the `Compare` function
object of `std::sort` or from the constructor of `T` in `std::make_shared`), it
is propagated, unchanged, to the caller.

### Exception objects

While objects of any complete type and cv pointers to void may be thrown as
exception objects, all standard library functions throw anonymous temporary
objects by value, and the types of those objects are derived (directly or
indirectly) from `std::exception`. User-defined exceptions usually follow this
pattern.[7][8][9]

To avoid unnecessary copying of the exception object and object slicing, the
best practice for catch clauses is to catch by reference.[10][11][12][13]

### External links

  H. Sutter (2004) "When and How to Use Exceptions" in Dr. Dobb's H. Sutter, A.
      Alexandrescu (2004), "C++ Coding Standards", Item 70 C++ Core Guidelines
      I.10: Use exceptions to signal a failure to perform a required task B.
      Stroustrup (2000), "The C++ Programming Language" Appendix E H. Sutter
      (2000) "Exceptional C++" D. Abrahams (2001) "Exception Safety in Generic
      Components" D. Abrahams (2001) "Error and Exception Handling" isocpp.org
      Super-FAQ "What should I throw?" C++ Core Guidelines E.14: Use
      purpose-designed user-defined types as exceptions (not built-in types) C++
      Core Guidelines E.15: Throw by value, catch exceptions from a hierarchy by
      reference S. Meyers (1996) "More Effective C++" Item 13 isocpp.org
      Super-FAQ "What should I catch?" H. Sutter, A. Alexandrescu (2004) "C++
      Coding Standards" Item 73

---
*Source: https://en.cppreference.com/w/cpp/language/exceptions*
