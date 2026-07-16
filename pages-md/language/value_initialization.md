# Value-initialization

This is the initialization performed when an object is constructed with an empty
initializer.

### Syntax

```text
T ()    (1)
new T ()    (2)
Class::Class(...) : member () { ... }    (3)
T object {};    (4)    (since C++11)
T {}    (5)    (since C++11)
new T {}    (6)    (since C++11)
Class::Class(...) : member {} { ... }    (7)    (since C++11)
```

### Explanation

Value-initialization is performed in these situations:

1,5) when a nameless temporary object is created with the initializer consisting
   of an empty pair of parentheses or braces(since C++11);

2,6) when an object with dynamic storage duration is created by a new-expression
   with the initializer consisting of an empty pair of parentheses or
   braces(since C++11);

3,7) when a non-static data member or a base class is initialized using a member
   initializer with an empty pair of parentheses or braces(since C++11);

4) when a named object (automatic, static, or thread-local) is declared with the
initializer consisting of a pair of braces.
*(since C++11)*

In all cases, if the empty pair of braces `{}` is used and `T` is an *aggregate
type*, aggregate-initialization is performed instead of value-initialization.

If `T` is a class type that has no default constructor but has a constructor
taking `std::initializer_list`, list-initialization is performed.
*(since C++11)*

The effects of value-initialization are:

1) if `T` is a class type with no default constructor or with a
   user-declared(until C++11)user-provided or deleted(since C++11) default
   constructor, the object is default-initialized;

2) if `T` is a class type with a default constructor that is not
   user-declared(until C++11)neither user-provided nor deleted(since C++11)
   (that is, it may be a class with an implicitly-defined or defaulted default
   constructor), the object is zero-initialized and the semantic constraints for
   default-initialization are checked, and if `T` has a non-trivial default
   constructor, the object is default-initialized;

3) if `T` is an array type, each element of the array is value-initialized;

4) otherwise, the object is zero-initialized.

### Notes

The syntax `T object();` does not initialize an object; it declares a function
that takes no arguments and returns `T`. The way to value-initialize a named
variable before C++11 was `T object = T();`, which value-initializes a temporary
and then copy-initializes the object: most compilers optimize out the copy in
this case.

References cannot be value-initialized.

As described in functional cast, the syntax `T()` (1) is prohibited for arrays,
while `T{}` (5) is allowed.

All standard containers (`std::vector`, `std::list`, etc.) value-initialize
their elements when constructed with a single `size_type` argument or when grown
by a call to `resize()`, unless their allocator customizes the behavior of
`construct`.

The standard specifies that zero-initialization is not performed when the class
has a user-provided or deleted default constructor, which implies that whether
said default constructor is selected by overload resolution is not considered.
All known compilers performs additional zero-initialization if a non-deleted
defaulted default constructor is selected.

```cpp
struct A
{
    A() = default;

    template<class = void>
    A(int = 0) {} // A has a user-provided default constructor, which is not selected

    int x;
};

constexpr int test(A a)
{
    return a.x; // the behavior is undefined if a's value is indeterminate
}

constexpr int zero = test(A());
// ill-formed: the parameter is not zero-initialized according to the standard,
// which results in undefined behavior that makes the program ill-formed in contexts
// where constant evaluation is required.
// However, such code is accepted by all known compilers.

void f()
{
    A a = A(); // not zero-initialized according to the standard
               // but implementations generate code for zero-initialization nonetheless
}
```

### Example

```cpp
#include <cassert>
#include <iostream>
#include <string>
#include <vector>

struct T1
{
    int mem1;
    std::string mem2;
    virtual void foo() {} // make sure T1 is not an aggregate
}; // implicit default constructor

struct T2
{
    int mem1;
    std::string mem2;
    T2(const T2&) {} // user-provided copy constructor
};                   // no default constructor

struct T3
{
    int mem1;
    std::string mem2;
    T3() {} // user-provided default constructor
};

std::string s{}; // class => default-initialization, the value is ""

int main()
{
    int n{};                // scalar => zero-initialization, the value is 0
    assert(n == 0);
    double f = double();    // scalar => zero-initialization, the value is 0.0
    assert(f == 0.0);
    int* a = new int[10](); // array => value-initialization of each element
    assert(a[9] == 0);      //          the value of each element is 0
    T1 t1{};                // class with implicit default constructor =>
    assert(t1.mem1 == 0);   //     t1.mem1 is zero-initialized, the value is 0
    assert(t1.mem2 == "");  //     t1.mem2 is default-initialized, the value is ""
//  T2 t2{};                // error: class with no default constructor
    T3 t3{};                // class with user-provided default constructor =>
    std::cout << t3.mem1;   //     t3.mem1 is default-initialized to indeterminate value
    assert(t3.mem2 == "");  //     t3.mem2 is default-initialized, the value is ""
    std::vector<int> v(3);  // value-initialization of each element
    assert(v[2] == 0);      // the value of each element is 0
    std::cout << '\n';
    delete[] a;
}
```

Possible output:

```text
42
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 178 | C++98 | there was no value-initialization; empty initializer invoked
      default- initialization (though `new T()` also performs
      zero-initialization) | empty initializer invoke value-initialization
  CWG 543 | C++98 | value-initialization for a class object without any
      user-provided constructors was equivalent to value- initializing each
      subobject (which need not zero- initialize a member with user-provided
      default constructor) | zero-initializes the entire object, then calls the
      default constructor
  CWG 1301 | C++11 | value-initialization of unions with deleted default
      constructors led to zero-initialization | they are default-initialized
  CWG 1368 | C++98 | any user-provided constructor caused zero-initialization to
      be skipped | only a user-provided default constructor skips
      zero-initialization
  CWG 1502 | C++11 | value-initializing a union without a user-provided default
      constructor only zero-initialized the object, despite default member
      initializers | performs default- initialization after zero-initialization
  CWG 1507 | C++98 | value-initialization for a class object without any
      user-provided constructors did not check the validity of the default
      constructor when the latter is trivial | the validity of trivial default
      constructor is checked

### See also

- default constructor
- `explicit`
- aggregate initialization
- list-initialization

---
*Source: https://en.cppreference.com/w/cpp/language/value_initialization*
