# Attribute specifier sequence(since C++11)

Introduces implementation-defined attributes for types, objects, code, etc.

### Syntax

```text
[[ attribute-list ]]    (since C++11)
[[ using attribute-namespace : attribute-list ]]    (since C++17)
```

where attribute-list is a comma-separated sequence of zero or more attribute ﻿s
(possibly ending with an ellipsis `...` indicating a pack expansion)

```text
identifier    (1)
attribute-namespace :: identifier    (2)
identifier ( argument-list ﻿(optional) )    (3)
attribute-namespace :: identifier ( argument-list ﻿(optional) )    (4)
```

where attribute-namespace is an identifier and argument-list is a sequence of
tokens where parentheses, brackets and braces are balanced (balanced-token-seq).

1) Simple attribute, such as `[[noreturn]]`.

2) Attribute with a namespace, such as `[[gnu::unused]]`.

3) Attribute with arguments, such as `[[deprecated("because")]]`.

4) Attribute with both a namespace and an argument list.

If `using namespace:` appears in the beginning of an attribute list, no other
attributes in the attribute list can specify a namespace: the namespace
specified in a using applies to them all:
```cpp
[[using CC: opt(1), debug]] // same as [[CC::opt(1), CC::debug]]
[[using CC: CC::opt(1)]] // error: cannot combine using and scoped attribute
```
*(since C++17)*

### Explanation

Attributes provide the unified standard syntax for implementation-defined
language extensions, such as the GNU and IBM language extensions
`__attribute__((...))`, Microsoft extension `__declspec()`, etc.

An attribute can be used almost everywhere in the C++ program, and can be
applied to almost everything: to types, to variables, to functions, to names, to
code blocks, to entire translation units, although each particular attribute is
only valid where it is permitted by the implementation: `[[expect_true]]` could
be an attribute that can only be used with an if, and not with a class
declaration. `[[omp::parallel()]]` could be an attribute that applies to a code
block or to a for loop, but not to the type int, etc (note these two attributes
are fictional examples, see below for the standard and some non-standard
attributes).

In declarations, attributes may appear both before the whole declaration and
directly after the name of the entity that is declared, in which case they are
combined. In most other situations, attributes apply to the directly preceding
entity.

The alignas specifier is a part of the attribute specifier sequence, although it
has different syntax. It may appear where the `[[...]]` attributes appear and
may mix with them (provided it is used where alignas is permitted).

Two consecutive left square bracket tokens (`[[`) may only appear when
introducing an attribute-specifier or inside an attribute argument.

```cpp
void f()
{
    int y[3];
    y[[] { return 0; }()] = 1;  // error
    int i [[cats::meow([[]])]]; // OK
}
```

Besides the standard attributes listed below, implementations may support
arbitrary non-standard attributes with implementation-defined behavior. All
attributes unknown to an implementation are ignored without causing an
error.(since C++17)

An attribute without attribute-namespace and an attribute-namespace whose name
is either `std` or `std` followed by one or more digits is reserved for future
standardization. That is, every non-standard attribute is in the
attribute-namespace provided by the implementation, e.g. `[[gnu::may_alias]]`,
`[[clang::trivial_abi]]`, and `[[msvc::noop_dtor]]`.
*(since C++20)*

### Standard attributes

The following attributes are defined by the C++ standard.

Standard attributes cannot be syntactially ignored: they cannot contain syntax
errors, must be applied to the correct target, and entities in the arguments
must be ODR-used.

Standard attributes cannot be semantically ignored either: the behavior with all
instances of a particular standard attribute removed would have been a
conforming behavior for the original program with the attribute present.

- **`[[noreturn]]`(C++11)** — indicates that the function does not return
  (attribute specifier)
- **`[[carries_dependency]]`(C++11)** — indicates that dependency chain in
  release-consume `std::memory_order` propagates in and out of the function
  (attribute specifier)
- **`[[deprecated]]`(C++14) `[[deprecated("reason")]]`(C++14)** — indicates that
  the use of the name or entity declared with this attribute is allowed, but
  discouraged for some reason (attribute specifier)
- **`[[fallthrough]]`(C++17)** — indicates that the fall through from the
  previous case label is intentional and should not be diagnosed by a compiler
  that warns on fall-through (attribute specifier)
- **`[[nodiscard]]`(C++17) `[[nodiscard("reason")]]`(C++20)** — encourages the
  compiler to issue a warning if the return value is discarded (attribute
  specifier)
- **`[[maybe_unused]]`(C++17)** — suppresses compiler warnings on unused
  entities, if any (attribute specifier)
- **`[[likely]]`(C++20) `[[unlikely]]`(C++20)** — indicates that the compiler
  should optimize for the case where a path of execution through a statement is
  more or less likely than any other path of execution (attribute specifier)
- **`[[no_unique_address]]`(C++20)** — indicates that a non-static data member
  need not have an address distinct from all other non-static data members of
  its class (attribute specifier)
- **`[[assume(expression)]]`(C++23)** — specifies that the *expression* will
  always evaluate to true at a given point (attribute specifier)
- **`[[optimize_for_synchronized]]`(TM TS)** — indicates that the function
  definition should be optimized for invocation from a synchronized statement
  (attribute specifier)

### Notes

The presence of each individual attribute on a given platform can be checked
with `__has_cpp_attribute` preprocessor macro.

  Feature-test macro | Value | Std | Feature
  `__cpp_attributes` | 200809L | (C++11) | Attributes
  `__cpp_namespace_attributes` | 201411L | (C++17) | Attributes for namespaces

### Example

```cpp
[[gnu::always_inline]] [[gnu::hot]] [[gnu::const]] [[nodiscard]]
inline int f(); // declare f with four attributes

[[gnu::always_inline, gnu::const, gnu::hot, nodiscard]]
int f(); // same as above, but uses a single attr specifier that contains four attributes

// C++17:
[[using gnu : const, always_inline, hot]] [[nodiscard]]
int f[[gnu::always_inline]](); // an attribute may appear in multiple specifiers

int f() { return 0; }

int main() {}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 2079 | C++11 | `[[` could not appear inside an attribute argument |
      allowed
  CWG 2538 | C++11 | it was unclear whether standard attributes can be
      syntactically ignored | prohibited
  CWG 2695 | C++11 | it was unclear whether standard attributes can be
      semantically ignored | prohibited
  P2156R1 | C++11 | every standard attribute was required to appear at most once
      in an attribute-list | not required

### See also

**`__has_cpp_attribute` - checks for the presence of an attribute**

**C documentation for Attributes specifier sequence**

### External links

  1. | Attributes in GCC. These attributes can be used as `[[gnu::...]]`, See
      SO.
  2. | Attributes in Clang.
  3. | Attributes in MSVC.

---
*Source: https://en.cppreference.com/w/cpp/language/attributes*
