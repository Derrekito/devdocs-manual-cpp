# typeid operator

Queries information of a type.

Used where the dynamic type of a polymorphic object must be known and for static
type identification.

### Syntax

```text
typeid ( type )    (1)
typeid ( expression )    (2)
```

The header `<typeinfo>` must be included or imported(since C++20) or a standard
library module imported(since C++23) before using `typeid`, otherwise the
program is ill-formed.

The typeid expression is an lvalue expression which refers to an object with
static storage duration, of const-qualified version of the polymorphic type
`std::type_info` or some type derived from it.

### Explanation

If the type of type or expression is a class type or a reference to a class
type, then that class type cannot be an incomplete type.

1) Refers to a `std::type_info` object representing the type type. If type is a
   reference type, the result refers to a `std::type_info` object representing
   the cv-unqualified version of the referenced type.

2) Examines the expression expression

   a) If expression is an lvalue(until C++11)a glvalue(since C++11) expression
      that identifies an object of a polymorphic type (that is, a class that
      declares or inherits at least one virtual function), the `typeid`
      expression evaluates the expression and then refers to the
      `std::type_info` object that represents the dynamic type of the
      expression. If expression is obtained by applying the unary `*` operator
      to a pointer and the pointer is a null pointer value, an exception of type
      `std::bad_typeid`(until C++14)a type matching handlers of type
      `std::bad_typeid`(since C++14) is thrown.

   b) If expression is not an lvalue(until C++11)a glvalue(since C++11)
      expression of polymorphic type, `typeid` does not evaluate the expression,
      and the `std::type_info` object it identifies represents the static type
      of the expression. Lvalue-to-rvalue, array-to-pointer, or
      function-to-pointer conversions are not performed. Temporary
      materialization, however, is (formally) performed for prvalue arguments:
      the argument must be destructible in the context in which the `typeid`
      expression appears.(since C++17)

If the type of type or expression is cv-qualified, the result of the `typeid`
refers to a `std::type_info` object representing the cv-unqualified type (that
is, `typeid(const T) == typeid(T)`).

If `typeid` is used on an object under construction or destruction (in a
destructor or in a constructor, including constructor's initializer list or
default member initializers), then the `std::type_info` object referred to by
this `typeid` represents the class that is being constructed or destroyed even
if it is not the most-derived class.

### Keywords

`typeid`

### Notes

When applied to an expression of polymorphic type, evaluation of a typeid
expression may involve runtime overhead (a virtual table lookup), otherwise
typeid expression is resolved at compile time.

It is unspecified whether the destructor for the object referred to by `typeid`
is executed at the end of the program.

There is no guarantee that the same `std::type_info` instance will be referred
to by all evaluations of the typeid expression on the same type, although they
would compare equal, `std::type_info::hash_code` of those `type_info` objects
would be identical, as would be their `std::type_index`.

```cpp
const std::type_info& ti1 = typeid(A);
const std::type_info& ti2 = typeid(A);

assert(&ti1 == &ti2); // not guaranteed
assert(ti1 == ti2); // guaranteed
assert(ti1.hash_code() == ti2.hash_code()); // guaranteed
assert(std::type_index(ti1) == std::type_index(ti2)); // guaranteed
```

### Example

The example showing output using one of the implementations where
`type_info::name` returns full type names; filter through c++filt -t if using
gcc or similar.

```cpp
#include <iostream>
#include <string>
#include <typeinfo>

struct Base {}; // non-polymorphic
struct Derived : Base {};

struct Base2 { virtual void foo() {} }; // polymorphic
struct Derived2 : Base2 {};

int main()
{
    int myint = 50;
    std::string mystr = "string";
    double *mydoubleptr = nullptr;

    std::cout << "myint has type: " << typeid(myint).name() << '\n'
              << "mystr has type: " << typeid(mystr).name() << '\n'
              << "mydoubleptr has type: " << typeid(mydoubleptr).name() << '\n';

    // std::cout << myint is a glvalue expression of polymorphic type; it is evaluated
    const std::type_info& r1 = typeid(std::cout << myint); // side-effect: prints 50
    std::cout << '\n' << "std::cout<<myint has type : " << r1.name() << '\n';

    // std::printf() is not a glvalue expression of polymorphic type; NOT evaluated
    const std::type_info& r2 = typeid(std::printf("%d\n", myint));
    std::cout << "printf(\"%d\\n\",myint) has type : " << r2.name() << '\n';

    // Non-polymorphic lvalue is a static type
    Derived d1;
    Base& b1 = d1;
    std::cout << "reference to non-polymorphic base: " << typeid(b1).name() << '\n';

    Derived2 d2;
    Base2& b2 = d2;
    std::cout << "reference to polymorphic base: " << typeid(b2).name() << '\n';

    try
    {
        // dereferencing a null pointer: okay for a non-polymorphic expression
        std::cout << "mydoubleptr points to " << typeid(*mydoubleptr).name() << '\n';
        // dereferencing a null pointer: not okay for a polymorphic lvalue
        Derived2* bad_ptr = nullptr;
        std::cout << "bad_ptr points to... ";
        std::cout << typeid(*bad_ptr).name() << '\n';
    }
    catch (const std::bad_typeid& e)
    {
        std::cout << " caught " << e.what() << '\n';
    }
}
```

Possible output:

```text
======== output from Clang ========
myint has type: i
mystr has type: NSt3__112basic_stringIcNS_11char_traitsIcEENS_9allocatorIcEEEE
mydoubleptr has type: Pd
50
std::cout<<myint has type : NSt3__113basic_ostreamIcNS_11char_traitsIcEEEE
printf("%d\n",myint) has type : i
reference to non-polymorphic base: 4Base
reference to polymorphic base: 8Derived2
mydoubleptr points to d
bad_ptr points to...  caught std::bad_typeid

======== output from MSVC ========
myint has type: int
mystr has type: class std::basic_string<char,struct std::char_traits<char>,class std::allocator<char> >
mydoubleptr has type: double * __ptr64
50
std::cout<<myint has type : class std::basic_ostream<char,struct std::char_traits<char> >
printf("%d\n",myint) has type : int
reference to non-polymorphic base: struct Base
reference to polymorphic base: struct Derived2
mydoubleptr points to double
bad_ptr points to...  caught Attempted a typeid of nullptr pointer!
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 492 | C++98 | when `typeid` is applied to a reference to cv-qualified
      type, the result represents the referenced type | the result represents
      the cv-unqualified referenced type
  CWG 1416 | C++98 | the wording regarding top-level cv-qualification might be
      misinterpreted | improved the wording

### See also

- **type_info** — contains some type's information, generated by the
  implementation. This is the class returned by the **`typeid`** operator.
  (class)

---
*Source: https://en.cppreference.com/w/cpp/language/typeid*
