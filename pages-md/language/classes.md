# Classes

A class is a user-defined type.

A class type is defined by class-specifier, which appears in decl-specifier-seq
of the declaration syntax. See class declaration for the syntax of the class
specifier.

A class can have the following kinds of members:

1) data members:

   a) non-static data members, including bit-fields.

   b) static data members

2) member functions:

   a) non-static member functions

   b) static member functions

3) nested types:

   a) nested classes and enumerations defined within the class definition

   b) aliases of existing types, defined with `typedef` or type alias (since
      C++11)declarations

   c) the name of the class within its own definition acts as a public member
      type alias of itself for the purpose of lookup (except when used to name a
      constructor): this is known as *injected-class-name*

4) enumerators from all unscoped enumerations defined within the class, or
   introduced by using-declarations or using-enum-declarations(since C++20)

5) member templates (variable templates, (since C++14)class templates or
   function templates) may appear in the body of any non-local
   class/struct/union.

All members are defined at once in the class definition, they cannot be added to
an already-defined class (unlike the members of namespaces)

A member of a class `T` cannot use `T` as its name if the member is

- a static data member,
- a member function,
- a member type,
- a member template,
- an enumerator of an enumeration (unless the enumeration is scoped)(since
  C++11), or
- a member of a member anonymous union.

However, a non-static data member may use the name `T` as long as there are no
user-declared constructors.

A class with at least one declared or inherited virtual member function is
*polymorphic*. Objects of this type are polymorphic objects and have runtime
type information stored as part of the object representation, which may be
queried with `dynamic_cast` and `typeid`. Virtual member functions participate
in dynamic binding.

A class with at least one declared or inherited pure virtual member function is
an abstract class. Objects of this type cannot be created.

A class with a constexpr constructor is a LiteralType: objects of this type can
be manipulated by constexpr functions at compile time.
*(since C++11)*

### Properties of classes

#### Trivially copyable class
A *trivially copyable class* is a class that
- has at least one eligible copy constructor, move constructor, copy assignment
  operator, or move assignment operator,
- each eligible copy constructor is trivial
- each eligible move constructor is trivial
- each eligible copy assignment operator is trivial
- each eligible move assignment operator is trivial, and
- has a non-deleted trivial destructor.
#### Trivial class
A *trivial class* is a class that
- is trivially copyable, and
- has one or more eligible default constructors such that each is trivial.
#### Standard-layout class
A *standard-layout class* is a class that
- has no non-static data members of type non-standard-layout class (or array of
  such types) or reference,
- has no virtual functions and no virtual base classes,
- has the same access control for all non-static data members,
- has no non-standard-layout base classes,
- only one class in the hierarchy has non-static data members, and
- Informally, none of the base classes has the same type as the first non-static
  data member. Or, formally: given the class as S, has no element of the set
  M(S) of types as a base class, where M(X) for a type X is defined as:
A *standard-layout struct* is a standard-layout class defined with the class
keyword `struct` or the class keyword `class`. A *standard-layout union* is a
standard-layout class defined with the class keyword `union`.
*(since C++11)*

#### Implicit-lifetime class

An *implicit-lifetime class* is a class that

- is an aggregate whose destructor is not user-declared(until
  C++11)user-provided(since C++11), or
- has at least one trivial eligible constructor and a trivial, non-deleted
  destructor.

Notes: the implicit-lifetime property is clarified by defect report P0593R6.

#### POD class

A *POD class* is a class that

- is an aggregate,
- has no user-declared copy assignment operator,
- has no user-declared destructor, and
- has no non-static data members of type non-POD class (or array of such types)
  or reference.
*(until C++11)*

- is a trivial class,
- is a standard-layout class, and
- has no non-static data members of type non-POD class (or array of such types).
*(since C++11)*

A *POD struct* is a non-union POD class. A *POD union* is a union that is a POD
class.

The usage of the POD property is deprecated. User codes should expect or require
the trivial or standard-layout property, or both of them.
*(since C++20)*

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 148 | C++98 | POD classes could not contain pointers to member, which are
      themselves POD (scalar) types | restriction removed
  CWG 383 | C++98 | copy assignment operators or destructors could be
      user-declared in POD classes if they are not defined | not allowed
  CWG 1363 | C++11 | a class that has both trivial default constructors and
      non-trivial default constructors at the same time could be trivial | it is
      non-trivial
  CWG 1496 | C++11 | a class that only has constructors that are all defined as
      deleted could be trivial | it is non-trivial
  CWG 1672 | C++11 | a class could be a standard-layout class if it has multiple
      empty base classes | it is not a standard-layout class
  CWG 1734 | C++11 | a trivially copyable class could not have non-trivial
      deleted copy/move constructors/assignment operators | can be trivial if
      deleted
  CWG 1813 | C++11 | a class was never a standard-layout class if it has a base
      class that inherits a non-static data member | it can be a standard-layout
      class
  CWG 1881 | C++11 | for a standard-layout class and its base classes, unnamed
      bit-fields might be declared in a different class declaring the data
      members | all non-static data members and bit-fields need to be first
      declared in the same class
  CWG 1909 | C++98 | a member template could have the same name as its class |
      prohibited
  CWG 2120 | C++11 | the definition of M(X) in determining a standard- layout
      class did not consider the case of a class whose first member is an array
      | addressed this case in the definition of M(X)
  CWG 2605 | C++98 | an implicit-lifetime class could have a user-provided
      destructor | prohibited

---
*Source: https://en.cppreference.com/w/cpp/language/classes*
