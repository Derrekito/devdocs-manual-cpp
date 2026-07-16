# constinit specifier (since C++20)

- `constinit` - asserts that a variable has static initialization, i.e. zero
  initialization and constant initialization, otherwise the program is
  ill-formed.

### Explanation

The constinit specifier declares a variable with static or thread storage
duration. If a variable is declared with constinit, its initializing declaration
must be applied with constinit. If a variable declared with constinit has
dynamic initialization (even if it is performed as static initialization), the
program is ill-formed. If no constinit declaration is reachable at the point of
the initializing declaration, the program is ill-formed, no diagnostic required.

constinit cannot be used together with constexpr. When the declared variable is
a reference, constinit is equivalent to `constexpr`. When the declared variable
is an object, constexpr mandates that the object must have static initialization
and constant destruction and makes the object const-qualified, however,
constinit does not mandate constant destruction and const-qualification. As a
result, an object of a type which has constexpr constructors and no constexpr
destructor (e.g. std::shared_ptr<T>) might be declared with constinit but not
constexpr.

```cpp
const char* g() { return "dynamic initialization"; }
constexpr const char* f(bool p) { return p ? "constant initializer" : g(); }

constinit const char* c = f(true);     // OK
// constinit const char* d = f(false); // error
```

constinit can also be used in a non-initializing declaration to tell the
compiler that a thread_local variable is already initialized, reducing overhead
that would otherwise be incurred by a hidden guard variable.

```cpp
extern thread_local constinit int x;
int f() { return x; } // no check of a guard variable needed
```

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_constinit` | 201907L | (C++20) | constinit

### Keywords

`constinit`

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 2543 | C++20 | the behavior was unclear if the variable declared with
      constinit is dynamically initialized as part of static initialization |
      the program is ill- formed in this case

### See also

- **`consteval` specifier(C++20)** — specifies that a function is an *immediate
  function*, that is, every call to the function must be in a constant
  evaluation
- **`constexpr` specifier(C++11)** — specifies that the value of a variable or
  function can be computed at compile time
- **constant expression** — defines an expression that can be evaluated at
  compile time
- **constant initialization** — sets the initial values of the static variables
  to a compile-time constant
- **zero initialization** — sets the initial value of an object to zero

---
*Source: https://en.cppreference.com/w/cpp/language/constinit*
