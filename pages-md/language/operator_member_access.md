# Member access operators

Accesses a member of its operand.

  Operator name | Syntax | Over​load​able | Prototype examples (for class T)
  Inside class definition | Outside class definition
  subscript | a[b] a[...] (since C++23) | a[...] | (since C++23) | Yes | R&
      T::operator[](S b); R& T::operator[](...); (since C++23) | R&
      T::operator[](...); | (since C++23) | N/A
  a[...] | (since C++23)
  R& T::operator[](...); | (since C++23)
  indirection | *a | Yes | R& T::operator*(); | R& operator*(T a);
  address-of | &a | Yes | R* T::operator&(); | R* operator&(T a);
  member of object | a.b | No | N/A | N/A
  member of pointer | a->b | Yes | R* T::operator->(); | N/A
  pointer to member of object | a.*b | No | N/A | N/A
  pointer to member of pointer | a->*b | Yes | R& T::operator->*(S b); | R&
      operator->*(T a, S b);
  **Notes** As with most user-defined overloads, return types should match
      return types provided by the built-in operators so that the user-defined
      operators can be used in the same manner as the built-ins. However, in a
      user-defined operator overload, any type can be used as return type
      (including void). One exception is operator->, which must return a pointer
      or another class with overloaded operator-> to be realistically usable.

### Explanation

Built-in *subscript* operator provides access to an object pointed-to by the
pointer or array operand.

Built-in *indirection* operator provides access to an object or function
pointed-to by the pointer operand.

Built-in *address-of* operator creates a pointer pointing to the object or
function operand.

*Member of object* and *pointer to member of object* operators provide access to
a data member or member function of the object operand.

Built-in *member of pointer* and *pointer to member of pointer* operators
provide access to a data member or member function of the class pointed-to by
the pointer operand.

#### Built-in subscript operator

The subscript operator expressions have the form

```text
expr1 ﻿[expr2 ﻿]    (1)
expr1 ﻿[{expr ﻿, ...}]    (2)    (since C++11)
expr1 ﻿[expr2 ﻿, expr ﻿, ...]    (3)    (since C++23)
```

1) For the built-in operator, one of the expressions (either expr1 or expr2)
   must be a glvalue of type “array of `T`” or a prvalue of type “pointer to
   `T`”, while the other expression (expr2 or expr1, respectively) must be a
   prvalue of unscoped enumeration or integral type. The result of this
   expression has the type `T`. expr2 cannot be an unparenthesized comma
   expression.(since C++23)

2) The form with brace-enclosed list inside the square brackets is only used to
   call an overloaded `operator[]`.

3) The form with comma-separated expression list inside the square brackets is
   only used to call an overloaded `operator[]`.

The built-in subscript expression `E1[E2]` is exactly identical to the
expression `*(E1 + E2)` except for its value category (see below) and evaluation
order(since C++17): the pointer operand (which may be a result of
array-to-pointer conversion, and which must point to an element of some array or
one past the end) is adjusted to point to another element of the same array,
following the rules of pointer arithmetic, and is then dereferenced.

When applied to an array, the subscript expression is an lvalue if the array is
an lvalue, and an xvalue if it isn't(since C++11).

When applied to a pointer, the subscript expression is always an lvalue.

The type `T` is not allowed to be an incomplete type, even if the size or
internal structure of `T` is never used, as in `&x[0]`.

Using an unparenthesized comma expression as second (right) argument of a
subscript operator is deprecated.
For example, `a[b, c]` is deprecated and `a[(b, c)]` is not.
*(since C++20)(until C++23)*

An unparenthesized comma expression cannot be second (right) argument of a
subscript operator. For example, `a[b, c]` is either ill-formed or equivalent to
`a.operator[](b, c)`.
Parentheses are needed to for using a comma expression as the subscript, e.g.,
`a[(b, c)]`.
*(since C++23)*

In overload resolution against user-defined operators, for every object type `T`
(possibly cv-qualified), the following function signature participates in
overload resolution:

```cpp
T& operator[](T*, std::ptrdiff_t);
T& operator[](std::ptrdiff_t, T*);
```

```cpp
#include <iostream>
#include <map>
#include <string>

int main()
{
    int a[4] = {1, 2, 3, 4};
    int* p = &a[2];
    std::cout << p[1] << p[-1] << 1[p] << (-1)[p] << '\n';

    std::map<std::pair<int, int>, std::string> m;
    m[{1, 2}] = "abc"; // uses the [{...}] version
}
```

Output:

```text
4242
```

#### Built-in indirection operator

The indirection operator expressions have the form

```text
*expr
```

The operand of the built-in indirection operator must be pointer to object or a
pointer to function, and the result is the lvalue referring to the object or
function to which expr points.

A pointer to (possibly cv-qualified) void cannot be dereferenced. Pointers to
other incomplete types can be dereferenced, but the resulting lvalue can only be
used in contexts that allow an lvalue of incomplete type, e.g. when initializing
a reference.

In overload resolution against user-defined operators, for every type `T` that
is either object type (possibly cv-qualified) or function type (not const- or
ref-qualified), the following function signature participates in overload
resolution:

```cpp
T& operator*(T*);
```

```cpp
#include <iostream>

int f() { return 42; }

int main()
{
    int n = 1;
    int* pn = &n;

    int& r = *pn; // lvalue can be bound to a reference
    int m = *pn;  // indirection + lvalue-to-rvalue conversion

    int (*fp)() = &f;
    int (&fr)() = *fp; // function lvalue can be bound to a reference

    [](...){}(r, m, fr); // removes possible "unused variable" warnings
}
```

#### Built-in address-of operator

The address-of operator expressions have the form

```text
&expr    (1)
&class ﻿::member    (2)
```

1) If the operand is an lvalue expression of some object or function type `T`,
   `operator&` creates and returns a prvalue of type `T*`, with the same cv
   qualification, that is pointing to the object or function designated by the
   operand. If the operand has incomplete type, the pointer can be formed, but
   if that incomplete type happens to be a class that defines its own
   `operator&`, it is unspecified whether the built-in or the overload is used.
   For the operands of type with user-defined `operator&`, `std::addressof` may
   be used to obtain the true pointer. Note that, unlike C99 and later C
   versions, there's no special case for the unary `operator&` applied to the
   result of the unary `operator*`.

If the operand is the name of an overloaded function, the address may be taken
   only if the overload can be resolved due to context. See Address of an
   overloaded function for details. If expr names an explicit object member
   function, expr must be a qualified identifier. Applying `&` to an unqualified
   identifier naming an explicit object member function is ill-formed. (since
   C++23)

2) If the operand is a qualified name of a non-static or variant member other
   than an explicit object member function(since C++23), e.g. `&C::member`, the
   result is a prvalue pointer to member function or pointer to data member of
   type `T` in class `C`. Note that neither `&member` nor `C::member` nor even
   `&(C::member)` may be used to initialize a pointer to member.

In overload resolution against user-defined operators, this operator does not
introduce any additional function signatures: built-in address-of operator does
not apply if there exists an overloaded operator& that is a viable function.

```cpp
void f(int) {}
void f(double) {}

struct A { int i; };
struct B { void f(); };

int main()
{
    int n = 1;
    int* pn = &n;    // pointer
    int* pn2 = &*pn; // pn2 == pn

    int A::* mp = &A::i;      // pointer to data member
    void (B::*mpf)() = &B::f; // pointer to member function

    void (*pf)(int) = &f; // overload resolution due to initialization context
//  auto pf2 = &f; // error: ambiguous overloaded function type
    auto pf2 = static_cast<void (*)(int)>(&f); // overload resolution due to cast
}
```

#### Built-in member access operators

The member access operator expressions have the form

```text
expr ﻿.template(optional) id-expr    (1)
expr ﻿->template(optional) id-expr    (2)
expr ﻿.pseudo-destructor    (3)
expr ﻿->pseudo-destructor    (4)
```

1) The expr must be an expression of complete class type `T`.

2) The expr must be an expression of pointer to complete class type `T*`.

3,4) The expr must be an expression of scalar type (see below).

expr is evaluated even if it is not necessary (e.g. when the second operand
names a static member).

id-expr is a name of (formally, an id-expression that names) a data member or
member function of `T` or of an unambiguous and accessible base class `B` of `T`
(e.g. `E1.E2` or `E1->E2`), optionally qualified (e.g. `E1.B::E2` or
`E1->B::E2`), optionally using template disambiguator (e.g. `E1.template E2` or
`E1->template E2`).

If a user-defined operator-> is called, operator-> is called again on the
resulting value, recursively, until an operator-> is reached that returns a
plain pointer. After that, built-in semantics are applied to that pointer.

The expression `E1->E2` is exactly equivalent to `(*E1).E2` for built-in types;
that is why the following rules address only `E1.E2`.

In the expression `E1.E2`:

1) if `E2` is a static data member:

- if `E2` is of reference type `T&` or `T&&`(since C++11), the result is an
  lvalue of type `T` designating the object or function to which the reference
  is bound,
- otherwise, given the type of `E2` as `T`, the result is an lvalue of type `T`
  designating that static data member.

Essentially, `E1` is evaluated and discarded in both cases;
2) if `E2` is a non-static data member:

- if `E2` is of reference type `T&` or `T&&`(since C++11), the result is an
  lvalue of type `T` designating the object or function to which the
  corresponding reference member of `E1` is bound,
- otherwise, if `E1` is an lvalue, the result is an lvalue designating that
  non-static data member of `E1`,
- otherwise (if `E1` is an rvalue(until C++17)xvalue (which may be materialized
  from prvalue)(since C++17)), the result is an rvalue(until C++11)xvalue(since
  C++11) designating that non-static data member of `E1`.

If `E2` is not a mutable member, the cv-qualification of the result is the union
of the cv-qualifications of `E1` and `E2`, otherwise (if `E2` is a mutable
member), it is the union of the volatile-qualifications of `E1` and `E2`;

3) if `E2` is a static member function, the result is an lvalue designating that
   static member function. Essentially, `E1` is evaluated and discarded in this
   case;

4) if `E2` is a non-static member function including a destructor, the result is
   a special kind of prvalue designating that non-static member function of `E1`
   that can only be used as the left-hand operand of a member function call
   operator, and for no other purpose;

5) if `E2` is a member enumerator, given the type of `E2` as `T`, the result is
   an rvalue(until C++11)a prvalue(since C++11) of type `T` whose value is the
   value of the enumerator;

6) if `E2` is a nested type, the program is ill-formed;

7) if `E1` has a ScalarType and `E2` is a `~` followed by the type name or
   decltype specifier designating the same type (minus cv-qualifications),
   optionally qualified, the result is a special kind of prvalue that can only
   be used as the left-hand operand of a function call operator, and for no
   other purpose. The resulting function call expression is called *pseudo
   destructor call*. It takes no arguments, returns void, evaluates `E1`, and
   ends the lifetime of its result object. This is the only case where the
   left-hand operand of `operator.` has non-class type. Allowing pseudo
   destructor call makes it possible to write code without having to know if a
   destructor exists for a given type.

operator. cannot be overloaded, and for `operator->`, in overload resolution
against user-defined operators, the built-in operator does not introduce any
additional function signatures: built-in operator-> does not apply if there
exists an overloaded operator-> that is a viable function.

```cpp
#include <cassert>
#include <iostream>
#include <memory>

struct P
{
    template<typename T>
    static T* ptr() { return new T; }
};

template<typename T>
struct A
{
    A(int n): n(n) {}

    int n;
    static int sn;

    int f() { return 10 + n; }
    static int sf() { return 4; }

    class B {};
    enum E {RED = 1, BLUE = 2};

    void g()
    {
        typedef int U;

        // keyword template needed for a dependent template member
        int* p = T().template ptr<U>();
        p->~U(); // U is int, calls int's pseudo destructor
        delete p;
    }
};

template<>
int A<P>::sn = 2;

struct UPtrWrapper
{
    std::unique_ptr<std::string> uPtr;
    std::unique_ptr<std::string>& operator->() { return uPtr; }
};

int main()
{
    A<P> a(1);
    std::cout << a.n << ' '
              << a.sn << ' '   // A::sn also works
              << a.f() << ' '
              << a.sf() << ' ' // A::sf() also works
//            << a.B << ' '    // error: nested type not allowed
              << a.RED << ' '; // enumerator

    UPtrWrapper uPtrWrap{std::make_unique<std::string>("wrapped")};
    assert(uPtrWrap->data() == uPtrWrap.operator->().operator->()->data());
}
```

Output:

```text
1 2 11 4 1
```

If `E2` is a non-static member and the result of `E1` is an object whose type is
not similar to the type of `E1`, the behavior is undefined:

```cpp
struct A { int i; };
struct B { int j; };
struct D : A, B {};

void f()
{
    D d;
    static_cast<B&>(d).j;      // OK, object expression designates the B subobject of d
    reinterpret_cast<B&>(d).j; // undefined behavior
}
```

#### Built-in pointer-to-member access operators

The member access operator expressions through pointers to members have the form

```text
lhs ﻿.*rhs    (1)
lhs ﻿->*rhs    (2)
```

1) lhs must be an expression of class type `T`.

2) lhs must be an expression of type pointer to class type `T*`.

rhs must be an rvalue of type pointer to member (data or function) of `T` or
pointer to member of an unambiguous and accessible base class `B` of `T`.

The expression `E1->*E2` is exactly equivalent to `(*E1).*E2` for built-in
types; that is why the following rules address only `E1.*E2`.

In the expression `E1.*E2`:

1) if `E2` is a pointer to data member,

- if `E1` is an lvalue, the result is an lvalue designating that data member,
- otherwise (if `E1` is an rvalue(until C++17)xvalue (which may be materialized
  from prvalue)(since C++17)), the result is an rvalue(until C++11)xvalue(since
  C++11) designating that data member;

2) if `E2` is a pointer to member function, the result is a special kind of
   prvalue designating that member function that can only be used as the
   left-hand operand of a member function call operator, and for no other
   purpose;

3) cv-qualification rules are the same as for member of object operator, with
   one additional rule: a pointer to member that refers to a mutable member
   cannot be used to modify that member in a const object;

4) if `E2` is a null pointer-to-member value, the behavior is undefined;

5) if the result `E1` is an object such that its type is not similar to the type
   of `E1`, or its most derived object does not contain the member to which `E2`
   refers, the behavior is undefined;

6) if `E1` is an rvalue and `E2` points to a member function with ref-qualifier
   `&`, the program is ill-formed unless the member function has the
   cv-qualifier `const` but not `volatile`(since C++20);

7) if `E1` is an lvalue and `E2` points to a member function with ref-qualifier
`&&`, the program is ill-formed.
*(since C++11)*

In overload resolution against user-defined operators, for every combination of
types `D`, `B`, `R`, where class type `B` is either the same class as `D` or an
unambiguous and accessible base class of `D`, and `R` is either an object or
function type, the following function signature participates in overload
resolution:

```cpp
R& operator->*(D*, R B::*);
```

where both operands may be cv-qualified, in which case the return type's
cv-qualification is the union of the cv-qualification of the operands.

```cpp
#include <iostream>

struct S
{
    S(int n) : mi(n) {}
    mutable int mi;
    int f(int n) { return mi + n; }
};

struct D : public S
{
    D(int n) : S(n) {}
};

int main()
{
    int S::* pmi = &S::mi;
    int (S::* pf)(int) = &S::f;

    const S s(7);
//  s.*pmi = 10; // error: cannot modify through mutable
    std::cout << s.*pmi << '\n';

    D d(7); // base pointers work with derived object
    D* pd = &d;
    std::cout << (d.*pf)(7) << ' '
              << (pd->*pf)(8) << '\n';
}
```

Output:

```text
7
14 15
```

### Standard library

Subscript operator is overloaded by many standard container classes:

- **operator[]** — accesses specific bit (public member function of
  `std::bitset<N>`)
- **operator[]** — provides indexed access to the managed array (public member
  function of `std::unique_ptr<T,Deleter>`)
- **operator[]** — accesses the specified character (public member function of
  `std::basic_string<CharT,Traits,Allocator>`)
- **operator[]** — access specified element (public member function of
  `std::array<T,N>`)
- **operator[]** — access specified element (public member function of
  `std::deque<T,Allocator>`)
- **operator[]** — access specified element (public member function of
  `std::vector<T,Allocator>`)
- **operator[]** — access or insert specified element (public member function of
  `std::map<Key,T,Compare,Allocator>`)
- **operator[]** — access or insert specified element (public member function of
  `std::unordered_map<Key,T,Hash,KeyEqual,Allocator>`)
- **operator[]** — accesses an element by index (public member function of
  `std::reverse_iterator<Iter>`)
- **operator[] (C++11)** — accesses an element by index (public member function
  of `std::move_iterator<Iter>`)
- **operator[]** — get/set valarray element, slice, or mask (public member
  function of `std::valarray<T>`)
- **operator[]** — returns specified sub-match (public member function of
  `std::match_results<BidirIt,Alloc>`)

The indirection and member operators are overloaded by many iterators and smart
pointer classes:

- **operator*operator->** — dereferences pointer to the managed object (public
  member function of `std::unique_ptr<T,Deleter>`)
- **operator*operator->** — dereferences the stored pointer (public member
  function of `std::shared_ptr<T>`)
- **operator*operator->** — accesses the managed object (public member function
  of `std::auto_ptr<T>`)
- **operator*** — dereferences the iterator (public member function of
  `std::raw_storage_iterator<OutputIt,T>`)
- **operator*operator->** — dereferences the decremented underlying iterator
  (public member function of `std::reverse_iterator<Iter>`)
- **operator*** — no-op (public member function of
  `std::back_insert_iterator<Container>`)
- **operator*** — no-op (public member function of
  `std::front_insert_iterator<Container>`)
- **operator*** — no-op (public member function of
  `std::insert_iterator<Container>`)
- **operator*operator-> (C++11)(C++11)(deprecated in C++20)** — accesses the
  pointed-to element (public member function of `std::move_iterator<Iter>`)
- **operator*operator->** — returns the current element (public member function
  of `std::istream_iterator<T,CharT,Traits,Distance>`)
- **operator*** — no-op (public member function of
  `std::ostream_iterator<T,CharT,Traits>`)
- **operator*** — obtains a copy of the current character (public member
  function of `std::istreambuf_iterator<CharT,Traits>`)
- **operator*** — no-op (public member function of
  `std::ostreambuf_iterator<CharT,Traits>`)
- **operator*operator->** — accesses the current match (public member function
  of `std::regex_iterator<BidirIt,CharT,Traits>`)
- **operator*operator->** — accesses current submatch (public member function of
  `std::regex_token_iterator<BidirIt,CharT,Traits>`)

No standard library classes overload operator&. The best known example of
overloaded operator& is the Microsoft COM class `CComPtr`, although it can also
appear in EDSLs such as boost.spirit.

No standard library classes overload operator->*. It was suggested that it could
be part of smart pointer interface, and in fact is used in that capacity by
actors in boost.phoenix, but is more common in EDSLs such as cpp.react.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_multidimensional_subscript` | 202110L | (C++23) | Multidimensional
      subscript operator

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 1213 | C++11 | subscripting an array rvalue resulted in lvalue |
      reclassified as xvalue
  CWG 1458 | C++98 | applying `&` to an lvalue of incomplete class type which
      declares `operator&` resulted in undefined behavior | it is unspecified
      which `&` is used
  CWG 1642 | C++98 | the rhs ﻿ in built-in pointer-to-member access operators
      could be an lvalue | can only be an rvalue
  CWG 1800 | C++98 | when applying `&` to a non-static data member of a member
      anonymous union, it was unclear whether the anonymous union take a part in
      the result type | the anonymous union is not included in the result type
  CWG 2614 | C++98 | the result of `E1.E2` was unclear if `E2` is a reference
      member or enumerator | made clear

### See also

Operator precedence

Operator overloading

  Common operators
  assignment | increment decrement | arithmetic | logical | comparison |
      **member access** | other
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

**C documentation for Member access operators**

---
*Source: https://en.cppreference.com/w/cpp/language/operator_member_access*
