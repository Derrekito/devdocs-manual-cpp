# Address of an overloaded function

Besides function-call expressions, where overload resolution takes place, the
name of an overloaded function may appear in the following 7 contexts:

  # | Context | Target
  1 | initializer in a declaration of an object or reference | the object or
      reference being initialized
  2 | on the right-hand-side of an assignment expression | the left-hand side of
      the assignment
  3 | as a function call argument | the function parameter
  4 | as a user-defined operator argument | the operator parameter
  5 | the `return` statement | the return type of a function
  6 | explicit cast or `static_cast` argument | the target type of a cast
  7 | non-type template argument | the type of the template parameter

In each context, the name of an overloaded function may be preceded by
address-of operator `&` and may be enclosed in a redundant set of parentheses.

In all these contexts, the function selected from the overload set is the
function whose type matches the pointer to function, reference to function, or
pointer to member function type that is expected by *target*.

The parameter types and the return type of the function must match the target
exactly. No implicit conversions are considered (e.g. a function returning a
pointer to derived won't get selected when initializing a pointer to function
returning a pointer to base).

If the function name names a function template, then, first, template argument
deduction is done, and if it succeeds, it produces a single template
specialization which is added to the set of overloads to consider. All functions
whose associated constraints are not satisfied are dropped from the set.(since
C++20) If more than one function from the set matches the target, and at least
one function is non-template, the template specializations are eliminated from
consideration. For any pair of non-template functions where one is more
constrained than another, the less constrained function is dropped from the
set(since C++20). If all remaining candidates are template specializations, less
specialized ones are removed if more specialized are available. If more than one
candidate remains after the removals, the program is ill-formed.

### Example

```cpp
int f(int) { return 1; }
int f(double) { return 2; }

void g(int(&f1)(int), int(*f2)(double)) { f1(0); f2(0.0); }

template<int(*F)(int)>
struct Templ {};

struct Foo
{
    int mf(int) { return 3; }
    int mf(double) { return 4; }
};

struct Emp
{
    void operator<<(int (*)(double)) {}
};

int main()
{
    // 1. initialization
    int (*pf)(double) = f; // selects int f(double)
    int (&rf)(int) = f; // selects int f(int)
    int (Foo::*mpf)(int) = &Foo::mf; // selects int mf(int)

    // 2. assignment
    pf = nullptr;
    pf = &f; // selects int f(double)

    // 3. function argument
    g(f, f); // selects int f(int) for the 1st argument
             // and int f(double) for the second

    // 4. user-defined operator
    Emp{} << f; //selects int f(double)

    // 5. return value
    auto foo = []() -> int (*)(int)
    {
        return f; // selects int f(int)
    };

    // 6. cast
    auto p = static_cast<int(*)(int)>(f); // selects int f(int)

    // 7. template argument
    Templ<f> t;  // selects int f(int)

    // prevent "unused variable" warnings as-if by [[maybe_unused]]
    [](...){}(pf, rf, mpf, foo, p, t);
}
```

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):
- C++17 standard (ISO/IEC 14882:2017):
- C++14 standard (ISO/IEC 14882:2014):
- C++11 standard (ISO/IEC 14882:2011):
- C++98 standard (ISO/IEC 14882:1998):

---
*Source: https://en.cppreference.com/w/cpp/language/overloaded_address*
