# Identifiers

An *identifier* is an arbitrarily long sequence of digits, underscores,
lowercase and uppercase Latin letters, and most Unicode characters.

The first character of a valid identifier must be one of the following:

- uppercase Latin letters A-Z
- lowercase Latin letters a-z
- underscore
- any Unicode character with the Unicode property XID_Start

Any other character of a valid identifier must be one of the following:

- digits 0-9
- uppercase Latin letters A-Z
- lowercase Latin letters a-z
- underscore
- any Unicode character with the Unicode property XID_Continue

The lists of characters with properties XID_Start and XID_Continue can be found
in DerivedCoreProperties.txt.

Identifiers are case-sensitive (lowercase and uppercase letters are distinct),
and every character is significant. Every identifier must conform to
Normalization Form C.

Note: Support of Unicode identifiers is limited in most implementations, e.g.
gcc (until 10).

### In declarations

An identifier can be used to name objects, references, functions, enumerators,
types, class members, namespaces, templates, template specializations, parameter
packs(since C++11) goto labels, and other entities, with the following
exceptions:

- the identifiers that are keywords cannot be used for other purposes;

- The only place they can be used as non-keywords is in an attribute-token (e.g.
  [[private]] is a valid attribute).
*(since C++11)*

- the identifiers that are alternative representations for certain operators and
  punctuators cannot be used for other purposes;

- the identifiers with special meaning (`final`, `import`, `module`(since C++20)
  and `override`) are used explicitly in a certain context rather than being
  regular identifiers; Unless otherwise specified, any ambiguity as to whether a
  given identifier has a special meaning is resolved to interpret the token as a
  regular identifier.
*(since C++11)*

- Identifiers that appear as a token or preprocessing token (i.e., not in
  user-defined-string-literal like `operator ""id`)(since C++11) of one of the
  following forms are reserved: identifiers with a double underscore anywhere;
  identifiers that begin with an underscore followed by an uppercase letter; in
  the global namespace, identifiers that begin with an underscore.

- In literal operators: literal suffix identifiers that do not start with an
  underscore are reserved for future standardization; literal suffix identifiers
  that contain double underscore are reserved for use by implementations.
*(since C++11)*

"Reserved" here means that the standard library headers #define or declare such
identifiers for their internal needs, the compiler may predefine non-standard
identifiers of that kind, and that name mangling algorithm may assume that some
of these identifiers are not in use. If the programmer uses such identifiers,
the program is ill-formed, no diagnostic required.

In addition, it's undefined behavior to #define or #undef certain names in a
translation unit, see reserved macro names for more details.

#### Zombie identifiers

As of C++14, some identifiers are removed from the C++ standard library. They
are listed in the list of zombie names.

However, these identifiers are still reserved for previous standardization in a
certain context. Removed member function names may not be used as a name for
function-like macros, and other removed member names may not be used as a name
for object-like macros in portable code.

### In expressions

An identifier that names a variable, a function, specialization of a
concept,(since C++20) or an enumerator can be used as an expression. The result
of an expression consisting of just the identifier is the entity named by the
identifier. The value category of the expression is *lvalue* if the identifier
names a function, a variable, a template parameter object(since C++20), or a
data member, and *rvalue*(until C++11)*prvalue*(since C++11) otherwise (e.g. an
enumerator is an rvalue(until C++11)a prvalue(since C++11) expression, a
specialization of a concept is a bool prvalue(since C++20)). The type of the
expression is determined as follows:

- If the entity named by the (unqualified) identifier is a local entity, and
  would result in an intervening lambda expression capturing it by copy if it
  were named outside of an unevaluated operand in the declarative region in
  which the identifier appears, then the type of the expression is the type of a
  class member access expression naming the non-static data member that would be
  declared for such a capture in the closure object of the innermost such
  intervening lambda expression.
```cpp
void f()
{
    float x, &r = x;
    [=]
    {
        decltype(x) y1;        // y1 has type float
        decltype((x)) y2 = y1; // y2 has type float const& because this lambda
                               // is not mutable and x is an lvalue
        decltype(r) r1 = y1;   // r1 has type float&
        decltype((r)) r2 = y2; // r2 has type float const&
    };
}
```
*(since C++11)*

- If the entity named is a template parameter object for a template parameter of
  type `T`, the type of the expression is const T.
*(since C++20)*

- Otherwise, the type of the expression is the same as the type of the entity
  named.

Within the body of a non-static member function, each identifier that names a
non-static member is implicitly transformed to a class member access expression
`this->member`.

#### Unqualified identifiers

Besides suitably declared identifiers, the following can be used in expressions
in the same role:

- an overloaded operator name in function notation, such as `operator+` or
  `operator new`;
- a user-defined conversion function name, such as `operator bool`;

- a user-defined literal operator name, such as `operator "" _km`;
*(since C++11)*

- a template name followed by its argument list, such as `MyTemplate<int>`;
- the character `~` followed by a class name, such as `~MyClass`;

- the character `~` followed by a decltype specifier, such as `~decltype(str)`.
*(since C++11)*

- the character `~` followed by a pack indexing specifier, such as
  `~pack...[0]`.
*(since C++26)*

Together with identifiers they are known as *unqualified id-expressions*.

#### Qualified identifiers

A *qualified id-expression* is an unqualified id-expression prepended by a scope
resolution operator `::`, and optionally, a sequence of any of the following
separated by scope resolution operators:

- a namespace name;
- a class name;

- an enumeration name;
- a `decltype` specifier denoting a class or enumeration type.
*(since C++11)*

- a pack indexing specifier specifier denoting a class or enumeration type.
*(since C++26)*

For example, the expression `std::string::npos` is an expression that names the
static member `npos` in the class `string` in namespace `std`. The expression
`::tolower` names the function `tolower` in the global namespace. The expression
`::std::cout` names the global variable `cout` in namespace `std`, which is a
top-level namespace. The expression `boost::signals2::connection` names the type
`connection` declared in namespace `signals2`, which is declared in namespace
`boost`.

The keyword `template` may appear in qualified identifiers as necessary to
disambiguate dependent template names.

See qualified lookup for the details of the name lookup for qualified
identifiers.

### Names

A *name* is the use of one of the following to refer to an entity:

- an identifier;
- an overloaded operator name in function notation (`operator+`, `operator
  new`);
- a user-defined conversion function name (`operator bool`);

- a user-defined literal operator name (`operator ""_km`);
*(since C++11)*

- a template name followed by its argument list (`MyTemplate<int>`).

Every name is introduced into the program by a declaration. A name used in more
than one translation unit may refer to the same or different entities, depending
on linkage.

When the compiler encounters an unknown name in a program, it associates it with
the declaration that introduced the name by means of name lookup, except for the
dependent names in template declarations and definitions (for those names, the
compiler determines whether they name a type, a template, or some other entity,
which may require explicit disambiguation).

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 1440 | C++11 | decltype expressions preceding `::` could denote any type |
      can only denote class or enumeration types
  CWG 1963 | C++11 | implementation-defined characters other than digits,
      non-digits and universal character names could be used in an identifier |
      prohibited
  CWG 2521 | C++11 | the identifier in user-defined-string-literal of a literal
      operator was reserved as usual | the rules are different

### See also

**C documentation for Identifiers**

---
*Source: https://en.cppreference.com/w/cpp/language/identifiers*
