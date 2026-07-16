# Increment/decrement operators

Increment/decrement operators increment or decrement the value of the object.

  Operator name | Syntax | Over​load​able | Prototype examples (for `class T`)
  Inside class definition | Outside class definition
  pre-increment | `++a` | Yes | `T& T::operator++();` | `T& operator++(T& a);`
  pre-decrement | `--a` | Yes | `T& T::operator--();` | `T& operator--(T& a);`
  post-increment | `a++` | Yes | `T T::operator++(int);` | `T operator++(T& a,
      int);`
  post-decrement | `a--` | Yes | `T T::operator--(int);` | `T operator--(T& a,
      int);`
  **Notes** Prefix versions of the built-in operators return *references* and
      postfix versions return *values*, and typical user-defined overloads
      follow the pattern so that the user-defined operators can be used in the
      same manner as the built-ins. However, in a user-defined operator
      overload, any type can be used as return type (including `void`). The
      `int` parameter is a dummy parameter used to differentiate between prefix
      and postfix versions of the operators. When the user-defined postfix
      operator is called, the value passed in that parameter is always zero,
      although it may be changed by calling the operator using function call
      notation (e.g., `a.operator++(2)` or `operator++(a, 2)`).

### Explanation

*Pre-increment* and *pre-decrement* operators increments or decrements the value
of the object and returns a reference to the result.

*Post-increment* and *post-decrement* creates a copy of the object, increments
or decrements the value of the object and returns the copy from before the
increment or decrement.

Using an lvalue of volatile-qualified non-class type as operand of built-in
version of these operators is deprecated.
*(since C++20)*

#### Built-in prefix operators

The prefix increment and decrement expressions have the form

```text
++ expr
-- expr
```

1) prefix increment (pre-increment)

2) prefix decrement (pre-decrement)

The operand expr of a built-in prefix increment or decrement operator must be a
modifiable (non-const) lvalue of non-boolean(since C++17) arithmetic type or
pointer to completely-defined object type. The expression `++x` is exactly
equivalent to `x += 1` for non-boolean operands(until C++17), and the expression
`--x` is exactly equivalent to `x -= 1`, that is, the prefix increment or
decrement is an lvalue expression that identifies the modified operand. All
arithmetic conversion rules and pointer arithmetic rules defined for arithmetic
operators apply and determine the implicit conversion (if any) applied to the
operand as well as the return type of the expression.

If the operand of the pre-increment operator is of type `bool`, it is set to
`true` (deprecated).(until C++17)

In overload resolution against user-defined operators, for every optionally
volatile-qualified arithmetic type `A` other than `bool`, and for every
optionally volatile-qualified pointer `P` to optionally cv-qualified object
type, the following function signatures participate in overload resolution:

```cpp
A& operator++(A&)
bool& operator++(bool&)  // (deprecated)(until C++17)
P& operator++(P&)
A& operator--(A&)
P& operator--(P&)
```

#### Built-in postfix operators

The postfix increment and decrement expressions have the form

```text
expr ++
expr --
```

1) postfix increment (post-increment)

2) postfix decrement (post-decrement)

The operand expr of a built-in postfix increment or decrement operator must be a
modifiable (non-const) lvalue of non-boolean(since C++17) arithmetic type or
pointer to completely-defined object type. The result is prvalue copy of the
original value of the operand. As a side-effect, the expression `x++` modifies
the value of its operand as if by evaluating `x += 1` for non-boolean
operands(until C++17), and the expression `x--` modifies the value of its
operand as if by evaluating `x -= 1`. All arithmetic conversion rules and
pointer arithmetic rules defined for arithmetic operators apply and determine
the implicit conversion (if any) applied to the operand as well as the return
type of the expression.

If the operand of the post-increment operator is of type `bool`, it is set to
`true` (deprecated).(until C++17)

In overload resolution against user-defined operators, for every optionally
volatile-qualified arithmetic type `A` other than `bool`, and for every
optionally volatile-qualified pointer `P` to optionally cv-qualified object
type, the following function signatures participate in overload resolution:

```cpp
A operator++(A&, int)
bool operator++(bool&, int)  // (deprecated)(until C++17)
P operator++(P&, int)
A operator--(A&, int)
P operator--(P&, int)
```

#### Example

```cpp
#include <iostream>

int main()
{
    int n1 = 1;
    int n2 = ++n1;
    int n3 = ++ ++n1;
    int n4 = n1++;
//  int n5 = n1++ ++;   // error
//  int n6 = n1 + ++n1; // undefined behavior
    std::cout << "n1 = " << n1 << '\n'
              << "n2 = " << n2 << '\n'
              << "n3 = " << n3 << '\n'
              << "n4 = " << n4 << '\n';
}
```

Output:

```text
n1 = 5
n2 = 2
n3 = 4
n4 = 4
```

### Notes

Because of the side-effects involved, built-in increment and decrement operators
must be used with care to avoid undefined behavior due to violations of
sequencing rules.

Because a temporary copy of the object is constructed during post-increment and
post-decrement, *pre-increment* or *pre-decrement* operators are usually more
efficient in contexts where the returned value is not used.

### Standard library

Increment and decrement operators are overloaded for many standard library
types. In particular, every LegacyIterator overloads operator++ and every
LegacyBidirectionalIterator overloads operator--, even if those operators are
no-ops for the particular iterator.

**overloads for arithmetic types**

- **operator++operator++(int)operator--operator--(int)** — increments or
  decrements the atomic value by one (public member function of
  `std::atomic<T>`)
- **operator++operator++(int)operator--operator--(int)** — increments or
  decrements the tick count (public member function of
  `std::chrono::duration<Rep,Period>`)

**overloads for iterator types**

- **operator++operator++(int)** — advances the iterator (public member function
  of `std::raw_storage_iterator<OutputIt,T>`)
-
  **operator++operator++(int)operator+=operator+operator--operator--(int)operator-=operator-**
  — advances or decrements the iterator (public member function of
  `std::reverse_iterator<Iter>`)
-
  **operator++operator++(int)operator+=operator+operator--operator--(int)operator-=operator-
  (C++11)** — advances or decrements the iterator (public member function of
  `std::move_iterator<Iter>`)
- **operator++operator++(int)** — no-op (public member function of
  `std::front_insert_iterator<Container>`)
- **operator++operator++(int)** — no-op (public member function of
  `std::back_insert_iterator<Container>`)
- **operator++operator++(int)** — no-op (public member function of
  `std::insert_iterator<Container>`)
- **operator++operator++(int)** — advances the iterator (public member function
  of `std::istream_iterator<T,CharT,Traits,Distance>`)
- **operator++operator++(int)** — no-op (public member function of
  `std::ostream_iterator<T,CharT,Traits>`)
- **operator++operator++(int)** — advances the iterator (public member function
  of `std::istreambuf_iterator<CharT,Traits>`)
- **operator++operator++(int)** — no-op (public member function of
  `std::ostreambuf_iterator<CharT,Traits>`)
- **operator++operator++(int)** — advances the iterator to the next match
  (public member function of `std::regex_iterator<BidirIt,CharT,Traits>`)
- **operator++operator++(int)** — advances the iterator to the next submatch
  (public member function of `std::regex_token_iterator<BidirIt,CharT,Traits>`)

### See also

Operator precedence

Operator overloading

  Common operators
  assignment | **increment decrement** | arithmetic | logical | comparison |
      member access | other
  `a = b a += b a -= b a *= b a /= b a %= b a &= b a |= b a ^= b a <<= b a >>=
      b` | `++a --a a++ a--` | `+a -a a + b a - b a * b a / b a % b ~a a & b a |
      b a ^ b a << b a >> b` | `!a a && b a || b` | `a == b a != b a < b a > b a
      <= b a >= b a <=> b` | `a[...] *a &a a->b a.b a->*b a.*b` | function call
  `a(...)`
  comma
  `a, b`
  conditional
  `a ? b : c`
  Special operators
  `static_cast` converts one type to another related type `dynamic_cast`
      converts within inheritance hierarchies `const_cast` adds or removes
      cv-qualifiers `reinterpret_cast` converts type to unrelated type C-style
      cast converts one type to another by a mix of `static_cast`, `const_cast`,
      and `reinterpret_cast` `new` creates objects with dynamic storage duration
      `delete` destructs objects previously created by the new expression and
      releases obtained memory area `sizeof` queries the size of a type
      `sizeof...` queries the size of a parameter pack (since C++11) `typeid`
      queries the type information of a type `noexcept` checks if an expression
      can throw an exception (since C++11) `alignof` queries alignment
      requirements of a type (since C++11)

**C documentation for Increment/decrement operators**

---
*Source: https://en.cppreference.com/w/cpp/language/operator_incdec*
