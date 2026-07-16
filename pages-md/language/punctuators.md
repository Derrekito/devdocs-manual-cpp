# Punctuation

These are the punctuation symbols in C++. The meaning of each symbol is detailed
in the linked pages.

### `{` `}`

- In a class/struct or union definition, delimit the member specification.
- In an enum definition, delimit the enumerator list.
- Delimit a compound statement. The compound statement may be part of
- In initialization, delimit the initializers. This kind of initialization is
  called list-initialization.(since C++11)
- In a namespace definition, delimit the namespace body.
- In a language linkage specification, delimit the declarations.
- In a requires-expression, delimit the requirements. (since C++20)
- In a compound requirement, delimit the expression. (since C++20)
- In an export declaration, delimit the declarations. (since C++20)

### `[` `]`

- Subscript operator; part of `operator[]` in operator overloading.
- Part of array declarator in a declaration or a type-id (e.g. in a new
  expression).
- Part of `new[]` operator in operator overloading (allocation function).
- Part of `delete[]` operator in delete expression and operator overloading
  (deallocation function).
- In a lambda expression, delimit the captures. (since C++11)
- In an attribute specifier, delimit the attributes. (since C++11)
- In a structured binding declaration, delimit the identifier list. (since
  C++17)

### `#`

- Introduce a preprocessing directive.
- The preprocessing operator for stringification.

### `##`

- The preprocessing operator for token pasting.

### `(` `)`

- In an expression, indicate grouping.
- Function call operator; part of `operator()` in operator overloading.
- In a function-style type cast, delimit the expression/initializers.
- In a `static_cast`, `const_cast`, `reinterpret_cast`, or `dynamic_cast`,
  delimit the expression.
- In a `typeid`, `sizeof`, `sizeof...`, `alignof`, or `noexcept`(since C++11)
  expression, delimit the operand.
- In a placement new expression, delimit the placement arguments.
- In a new expression, optionally delimit the type-id.
- In a new expression, delimit the initializers.
- In a C-style cast, delimit the type-id.
- In a declaration or a type-id, indicate grouping.
- Delimit the parameter list in
- In direct-initialization, delimit the initializers.
- In an asm declaration, delimit the string literal.
- In a member initializer list, delimit the initializers to a base or member.
- In an `if` (including `constexpr` `if`)(since C++17), `switch`, `while`,
  `do-while`, or `for`(including range-based `for`)(since C++11) statement,
  delimit the controlling clause.
- In a catch clause, delimit the parameter declaration.
- In a function-like macro definition, delimit the macro parameters.
- In a function-like macro invocation, delimit the macro arguments or prevent
  commas from being interpreted as argument separators.
- Part of a `defined`, `__has_include`(since C++17), `__has_cpp_attribute`(since
  C++20) preprocessing operator.
- In a `static_assert` declaration, delimit the operands. (since C++11)
- In a `decltype` specifier, `noexcept` specifier, `alignas` specifier,
  conditional `explicit` specifier(since C++20), delimit the operand. (since
  C++11)
- In an attribute, delimit the attribute arguments. (since C++11)
- Part of `decltype(auto)` specifier. (since C++14)
- Delimit a fold expression. (since C++17)
- Part of `__VA_OPT__` replacement in a variadic macro definition. (since C++20)

### `;`

- Indicate the end of
- Separate the second and third clauses of a for statement.

### `:`

- Part of conditional operator.
- Part of label declaration.
- In the class-head of a class definition, introduce the base-clause.
- Part of access specifier in member specification.
- In a bit-field member declaration, introduce the width.
- In a constructor definition, introduce the member initializer list.
- In a range-based for statement, separate the range-declaration and the
  range-initializer. (since C++11)
- Introduce an enum base, which specifies the underlying type of the enum.
  (since C++11)
- In an attribute specifier, separate the attribute-using-prefix and the
  attribute list. (since C++17)
- In a module declaration or import declaration of module partition, introduce
  the module partition name. (since C++20)
- Part of a private module fragment introducer (`module :private;`). (since
  C++20)

### `...`

- In the parameter list of a function declaratoror lambda expression(since
  C++11)or user-defined deduction guide(since C++17), signify a variadic
  function.
- In a catch-clause, signify catch-all handler.
- In a macro definition, signify a variadic macro. (since C++11)
- Indicate pack declaration and expansion. (since C++11)

### `?`

- Part of conditional operator.

### `::`

- Scope resolution operator in
- In an attribute, indicate attribute scope. (since C++11)
- Part of nested namespace definition. (since C++17)

### `.`

- Member access operator.
- In aggregate initialization, introduce a designator. (since C++20)
- Part of module name or module partition name. (since C++20)

### `.*`

- Pointer-to-member access operator.

### `->`

- Member access operator; part of `operator->` in operator overloading.
- In a function declarator or lambda expression, introduce the trailing return
  type. (since C++11)
- In a user-defined deduction guide, introduce the result type. (since C++17)
- In a compound requirement, introduce the return type requirement. (since
  C++20)

### `->*`

- Pointer-to-member access operator; part of `operator->*` in operator
  overloading.

### `~`

- Unary complement operator (a.k.a. bitwise not operator); part of `operator~`
  in operator overloading.
- Part of an id-expression to name a destructor or pseudo-destructor.

### `!`

- Logical not operator; part of `operator!` in operator overloading.
- Part of reversed variant of a consteval `if` statement. (since C++23)

### `+`

- Unary plus operator; part of `operator+` in operator overloading.
- Binary plus operator; part of `operator+` in operator overloading.

### `-`

- Unary minus operator; part of `operator-` in operator overloading.
- Binary minus operator; part of `operator-` in operator overloading.

### `*`

- Indirection operator; part of `operator*` in operator overloading.
- Multiplication operator; part of `operator*` in operator overloading.
- Pointer operator or part of pointer-to-member operator in a declarator or in a
  type-id.
- Part of `*this` in a lambda capture list, to capture the current object by
  copy. (since C++17)

### `/`

- Division operator; part of `operator/` in operator overloading.

### `%`

- Modulo operator; part of `operator%` in operator overloading.

### `^`

- Bitwise xor operator; part of `operator^` in operator overloading.

### `&`

- Address-of operator; part of `operator&` in operator overloading.
- Bitwise and operator; part of `operator&` in operator overloading.
- Lvalue-reference operator in a declarator or in a type-id.
- In a lambda capture, indicate by-reference capture. (since C++11)
- Ref-qualifier in member function declaration. (since C++11)

### `|`

- Bitwise or operator; part of `operator|` in operator overloading.

### `=`

- Simple assignment operator; part of `operator=` in operator overloading, which
  might be a special member function (copy assignment operatoror move assignment
  operator(since C++11)).
- In initialization, indicate copy-initializationor
  copy-list-initialization(since C++11).
- In a function declaration, introduce a default argument.
- In a template parameter list, introduce a default template argument.
- In a namespace alias definition, separate the alias and the aliased namespace.
- In an enum definition, introduce the value of enumerator.
- Part of pure-specifier in a pure virtual function declaration.
- Capture default in lambda capture, to indicate by-copy capture. (since C++11)
- Part of defaulted definition (`=default;`) or deleted definition (`=delete;`)
  in function definition. (since C++11)
- In a type alias declaration, separate the alias and the aliased type. (since
  C++11)
- In a concept definition, separate the concept name and the constraint
  expression. (since C++20)

### `+=`

- Compound assignment operator; part of `operator+=` in operator overloading.

### `-=`

- Compound assignment operator; part of `operator-=` in operator overloading.

### `*=`

- Compound assignment operator; part of `operator*=` in operator overloading.

### `/=`

- Compound assignment operator; part of `operator/=` in operator overloading.

### `%=`

- Compound assignment operator; part of `operator%=` in operator overloading.

### `^=`

- Compound assignment operator; part of `operator^=` in operator overloading.

### `&=`

- Compound assignment operator; part of `operator&=` in operator overloading.

### `|=`

- Compound assignment operator; part of `operator|=` in operator overloading.

### `==`

- Equality operator; part of `operator==` in operator overloading.

### `!=`

- Inequality operator; part of `operator!=` in operator overloading.

### `<`

- Less-than operator; part of `operator<` in operator overloading.
- In a `static_cast`, `const_cast`, `reinterpret_cast`, or `dynamic_cast`,
  introduce the type-id.
- Introduce a template argument list.
- Introduce a template parameter list in
- Part of `template<>` in template specialization declaration.
- Introduce a header name in

### `>`

- Greater-than operator; part of `operator>` in operator overloading.
- `static_cast`, `const_cast`, `reinterpret_cast`, or `dynamic_cast`, indicate
  the end of type-id.
- Indicate the end of a template argument list.
- Indicate the end of a template parameter list in
- Part of `template<>` in template specialization declaration.
- Indicate the end of a header name in

### `<=`

- Less-than-or-equal-to operator; part of `operator<=` in operator overloading.

### `>=`

- Greater-than-or-equal-to operator; part of `operator>=` in operator
  overloading.

### `<=>` (since C++20)

- Three-way comparison (spaceship) operator; part of `operator<=>` in operator
  overloading.

### `&&`

- Logical and operator; part of `operator&&` in operator overloading.
- Rvalue-reference operator in a declarator or in a type-id. (since C++11)
- Ref-qualifier in member function declaration. (since C++11)

### `||`

- Logical or operator; part of `operator||` in operator overloading.

### `<<`

- Bitwise shift operator; part of `operator<<` in operator overloading (bitwise
  operator or stream insertion operator).

### `>>`

- Bitwise shift operator; part of `operator>>` in operator overloading (bitwise
  operator or stream extraction operator).
- Can be reparsed as two `>` in a `static_cast`, `const_cast`,
  `reinterpret_cast`, or `dynamic_cast`, a template argument list, or a template
  parameter list. (since C++11)

### `<<=`

- Compound assignment operator; part of `operator<<=` in operator overloading.

### `>>=`

- Compound assignment operator; part of `operator>>=` in operator overloading.

### `++`

- Increment operator; part of `operator++` in operator overloading.

### `--`

- Decrement operator; part of `operator--` in operator overloading.

### `,`

- Comma operator; part of `operator,` in operator overloading.
- List separator in
- In a `static_assert` declaration, separate the arguments. (since C++11)

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):
- C++17 standard (ISO/IEC 14882:2017):
- C++14 standard (ISO/IEC 14882:2014):
- C++11 standard (ISO/IEC 14882:2011):
- C++03 standard (ISO/IEC 14882:2003):
- C++98 standard (ISO/IEC 14882:1998):

### See also

- **Alternative representations** — alternative spellings for certain operators

**C documentation for Punctuation**

---
*Source: https://en.cppreference.com/w/cpp/language/punctuators*
