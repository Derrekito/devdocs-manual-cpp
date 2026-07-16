# Declarations

*Declarations* are how names are introduced (or re-introduced) into the C++
program. Not all declarations actually declare anything, and each kind of entity
is declared differently. Definitions are declarations that are sufficient to use
the entity identified by the name.

A declaration is one of the following:

- Function definition
- Template declaration (including Partial template specialization)
- Explicit template instantiation
- Explicit template specialization
- Namespace definition
- Linkage specification

- Attribute declaration (attr `;`)
*(since C++11)*

- Empty declaration (`;`)
- A function declaration without a decl-specifier-seq:

```text
attr ﻿(optional) declarator ;
attr    -    (since C++11) sequence of any number of attributes
declarator    -    a function declarator
type alias declaration    (since C++11)
using-enum-declaration    (since C++20)
static_assert declaration opaque enum declaration    (since C++11)
decl-specifier-seq init-declarator-list ﻿(optional) ;    (1)
attr decl-specifier-seq init-declarator-list;    (2)
attr    -    (since C++11) sequence of any number of attributes
decl-specifier-seq    -    sequence of specifiers (see below)
init-declarator-list    -    comma-separated list of declarators with optional initializers. init-declarator-list is optional when declaring a named class/struct/union or a named enumeration
the inline specifier is also allowed on variable declarations.    (since C++17)
the constexpr specifier, only allowed in variable definitions, function and function template declarations, and the declaration of static data members of literal type.    (since C++11)
the consteval specifier, only allowed in function and function template declarations. the constinit specifier, only allowed in declaration of a variable with static or thread storage duration. At most one of the constexpr, consteval, and constinit specifiers is allowed to appear in a decl-specifier-seq.    (since C++20)
auto decltype specifier    (since C++11)
pack indexing specifier    (since C++26)
template name without template arguments (optionally qualified): see class template argument deduction    (since C++17)
long can be combined with long.    (since C++11)
declarator initializer ﻿(optional)    (1)
declarator requires-clause    (2)    (since C++20)
declarator    -    the declarator
initializer    -    optional initializer (except where required, such as when initializing references or const objects). See Initialization for details.
requires-clause    -    a requires-clause, which adds a constraint to a function declaration
unqualified-id attr ﻿(optional)    (1)
qualified-id attr ﻿(optional)    (2)
... identifier attr ﻿(optional)    (3)    (since C++11)
* attr ﻿(optional) cv ﻿(optional) declarator    (4)
nested-name-specifier * attr ﻿(optional) cv ﻿(optional) declarator    (5)
& attr ﻿(optional) declarator    (6)
&& attr ﻿(optional) declarator    (7)    (since C++11)
noptr-declarator [ constexpr ﻿(optional) ] attr ﻿(optional)    (8)
noptr-declarator ( parameter-list ) cv ﻿(optional) ref ﻿ ﻿(optional) except ﻿(optional) attr ﻿(optional)    (9)
In all cases, attr is an optional sequence of attributes. When appearing immediately after the identifier, it applies to the object being declared.    (since C++11)
CWG 482    C++98    the declarators of redeclarations could not be qualified    qualified declarators allowed
CWG 569    C++98    a single standalone semicolon was not a valid declaration    it is an empty declaration,which has no effect
CWG 1830    C++98    repetition of a function specifier in a decl-specifier-seq was allowed    repetition is forbidden
C documentation for Declarations
```

---
*Source: https://en.cppreference.com/w/cpp/language/declarations*
