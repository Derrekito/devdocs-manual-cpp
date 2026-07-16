# Copy elision

Omits copy and move(since C++11) constructors, resulting in zero-copy
pass-by-value semantics.

### Explanation

#### Prvalue semantics ("guaranteed copy elision")
Since C++17, a prvalue is not materialized until needed, and then it is
constructed directly into the storage of its final destination. This sometimes
means that even when the language syntax visually suggests a copy/move (e.g.
copy initialization), no copy/move is performed — which means the type need not
have an accessible copy/move constructor at all. Examples include:
- Initializing the returned object in a return statement, when the operand is a
  prvalue of the same class type (ignoring cv-qualification) as the function
  return type:
```cpp
T f()
{
    return U(); // constructs a temporary of type U,
                // then initializes the returned T from the temporary
}
T g()
{
    return T(); // constructs the returned T directly; no move
}
```
The destructor of the type returned must be accessible at the point of the
return statement and non-deleted, even though no T object is destroyed.
- In the initialization of an object, when the initializer expression is a
  prvalue of the same class type (ignoring cv-qualification) as the variable
  type:
```cpp
T x = T(T(f())); // x is initialized by the result of f() directly; no move
```
This can only apply when the object being initialized is known not to be a
potentially-overlapping subobject:
```cpp
struct C { /* ... */ };
C f();
struct D;
D g();
struct D : C
{
    D() : C(f()) {}    // no elision when initializing a base-class subobject
    D(int) : D(g()) {} // no elision because the D object being initialized might
                       // be a base-class subobject of some other class
};
```
Note: This rule does not specify an optimization, and the Standard does not
formally describe it as "copy elision" (because nothing is being elided).
Instead, the C++17 core language specification of prvalues and temporaries is
fundamentally different from that of earlier C++ revisions: there is no longer a
temporary to copy/move from. Another way to describe C++17 mechanics is
"unmaterialized value passing" or "deferred temporary materialization": prvalues
are returned and used without ever materializing a temporary.
*(since C++17)*

#### Non-mandatory copy/move(since C++11) elision

Under the following circumstances, the compilers are permitted, but not required
to omit the copy and move(since C++11) construction of class objects even if the
copy/move(since C++11) constructor and the destructor have observable
side-effects. The objects are constructed directly into the storage where they
would otherwise be copied/moved to. This is an optimization: even when it takes
place and the copy/move(since C++11) constructor is not called, it still must be
present and accessible (as if no optimization happened at all), otherwise the
program is ill-formed:

- In a return statement, when the operand is the name of a non-volatile object
  with automatic storage duration, which isn't a function parameter or a catch
  clause parameter, and which is of the same class type (ignoring
  cv-qualification) as the function return type. This variant of copy elision is
  known as NRVO, "named return value optimization."

- In the initialization of an object, when the source object is a nameless
  temporary and is of the same class type (ignoring cv-qualification) as the
  target object. When the nameless temporary is the operand of a return
  statement, this variant of copy elision is known as URVO, "unnamed return
  value optimization."
*(until C++17)*

URVO is mandatory and no longer considered a form of copy elision; see above.
*(since C++17)*

- In a throw-expression, when the operand is the name of a non-volatile object
  with automatic storage duration, which isn't a function parameter or a catch
  clause parameter, and whose scope does not extend past the innermost try-block
  (if there is a try-block).
- In a catch clause, when the argument is of the same type (ignoring
  cv-qualification) as the exception object thrown, the copy of the exception
  object is omitted and the body of the catch clause accesses the exception
  object directly, as if caught by reference (there cannot be a move from the
  exception object because it is always an lvalue). This is disabled if such
  copy elision would change the observable behavior of the program for any
  reason other than skipping the copy constructor and the destructor of the
  catch clause argument (for example, if the catch clause argument is modified,
  and the exception object is rethrown with `throw`).
*(since C++11)*

- In coroutines, copy/move of the parameter into coroutine state may be elided
  where this does not change the behavior of the program other than by omitting
  the calls to the parameter's constructor and destructor. This can take place
  if the parameter is never referenced after a suspension point or when the
  entire coroutine state was never heap-allocated in the first place.
*(since C++20)*

When copy elision occurs, the implementation treats the source and target of the
omitted copy/move(since C++11) operation as simply two different ways of
referring to the same object, and the destruction of that object occurs at the
later of the times when the two objects would have been destroyed without the
optimization (except that, if the parameter of the selected constructor is an
rvalue reference to object type, the destruction occurs when the target would
have been destroyed)(since C++11).

Multiple copy elisions may be chained to eliminate multiple copies.

- In constant expression and constant initialization, copy elision is never
  performed.
```cpp
struct A
{
    void* p;
    constexpr A() : p(this) {}
    A(const A&); // Disable trivial copyability
};
constexpr A a;  // OK: a.p points to a
constexpr A f()
{
    A x;
    return x;
}
constexpr A b = f(); // error: b.p would be dangling and point to the x inside f
constexpr A c = A(); // (until C++17) error: c.p would be dangling and point to a temporary
                     // (since C++17) OK: c.p points to c; no temporary is involved
```
*(since C++11)*

### Notes

Copy elision is the only allowed form of optimization(until C++14) one of the
two allowed forms of optimization, alongside allocation elision and
extension,(since C++14) that can change observable side-effects. Because some
compilers do not perform copy elision in every situation where it is allowed
(e.g., in debug mode), programs that rely on the side-effects of copy/move
constructors and destructors are not portable.

In a return statement or a throw-expression, if the compiler cannot perform copy
elision but the conditions for copy elision are met, or would be met except that
the source is a function parameter, the compiler will attempt to use the move
constructor even if the source operand is designated by an lvalue(until C++23)
the source operand will be treated as an rvalue(since C++23); see return
statement for details.
*(since C++11)*

  Feature-test macro | Value | Std | Feature
  `__cpp_guaranteed_copy_elision` | 201606L | (C++17) | Guaranteed copy elision
      through simplified value categories

### Example

```cpp
#include <iostream>

struct Noisy
{
    Noisy() { std::cout << "constructed at " << this << '\n'; }
    Noisy(const Noisy&) { std::cout << "copy-constructed\n"; }
    Noisy(Noisy&&) { std::cout << "move-constructed\n"; }
    ~Noisy() { std::cout << "destructed at " << this << '\n'; }
};

Noisy f()
{
    Noisy v = Noisy(); // (until C++17) copy elision initializing v from a temporary;
                       //               the move constructor may be called
                       // (since C++17) "guaranteed copy elision"
    return v; // copy elision ("NRVO") from v to the result object;
              // the move constructor may be called
}

void g(Noisy arg)
{
    std::cout << "&arg = " << &arg << '\n';
}

int main()
{
    Noisy v = f(); // (until C++17) copy elision initializing v from the result of f()
                   // (since C++17) "guaranteed copy elision"

    std::cout << "&v = " << &v << '\n';

    g(f()); // (until C++17) copy elision initializing arg from the result of f()
            // (since C++17) "guaranteed copy elision"
}
```

Possible output:

```text
constructed at 0x7fffd635fd4e
&v = 0x7fffd635fd4e
constructed at 0x7fffd635fd4f
&arg = 0x7fffd635fd4f
destructed at 0x7fffd635fd4f
destructed at 0x7fffd635fd4e
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 1967 | C++11 | when copy elision is done using a move constructor, the
      lifetime of the moved-from object was still considered | not considered
  CWG 2022 | C++11 | copy elision was optional during constant evaluation |
      mandatory during constant evaluation
  CWG 2278 | C++11 | copy elision was mandatory during constant evaluation |
      forbidden during constant evaluation
  CWG 2426 | C++17 | destructor not required when returning a prvalue |
      destructor is potentially invoked

### See also

- copy initialization
- copy constructor
- move constructor

---
*Source: https://en.cppreference.com/w/cpp/language/copy_elision*
