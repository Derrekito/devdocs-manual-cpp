# std::uncaught_exception, std::uncaught_exceptions

```cpp
bool uncaught_exception() throw();  // (until C++11)
bool uncaught_exception() noexcept;  // (since C++11) (deprecated in C++17) (removed in C++20)
int uncaught_exceptions() noexcept;  // (2) (since C++17)
```

1) Detects if the current thread has a live exception object, that is, an
   exception has been thrown or rethrown and not yet entered a matching catch
   clause, `std::terminate` or `std::unexpected`. In other words,
   `std::uncaught_exception` detects if stack unwinding is currently in
   progress.

2) Detects how many exceptions in the current thread have been thrown or
   rethrown and not yet entered their matching catch clauses.

Sometimes it's safe to throw an exception even while `std::uncaught_exception()
== true`. For example, if stack unwinding causes an object to be destructed, the
destructor for that object could run code that throws an exception as long as
the exception is caught by some catch block before escaping the destructor.

### Parameters

(none)

### Return value

1) `true` if stack unwinding is currently in progress in this thread, `false`
   otherwise.

2) The number of uncaught exception objects in the current thread.

### Notes

An example where int-returning `uncaught_exceptions` is used is the boost.log
library: the expression `BOOST_LOG(logger) << foo();` first creates a guard
object and records the number of uncaught exceptions in its constructor. The
output is performed by the guard object's destructor unless `foo()` throws (in
which case the number of uncaught exceptions in the destructor is greater than
what the constructor observed).

`std::experimental::scope_fail` and `std::experimental::scope_success` in LFTS
v3 rely on the functionality of `uncaught_exceptions`, because their destructors
need to do different things that depend on whether is called during stack
unwinding.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_uncaught_exceptions` | 201411L | (C++17) |
      `std::uncaught_exceptions`

### Example

```cpp
#include <exception>
#include <iostream>
#include <stdexcept>

struct Foo
{
    char id{'?'};
    int count = std::uncaught_exceptions();

    ~Foo()
    {
        count == std::uncaught_exceptions()
            ? std::cout << id << ".~Foo() called normally\n"
            : std::cout << id << ".~Foo() called during stack unwinding\n";
    }
};

int main()
{
    Foo f{'f'};

    try
    {
        Foo g{'g'};
        std::cout << "Exception thrown\n";
        throw std::runtime_error("test exception");
    }
    catch (const std::exception& e)
    {
        std::cout << "Exception caught: " << e.what() << '\n';
    }
}
```

Possible output:

```text
Exception thrown
g.~Foo() called during stack unwinding
Exception caught: test exception
f.~Foo() called normally
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 70 | C++98 | the exception specification of `uncaught_exception()` was
      missing | specified as `throw()`

### See also

- **terminate** — function called when exception handling fails (function)
- **exception_ptr (C++11)** — shared pointer type for handling exception objects
  (typedef)
- **current_exception (C++11)** — captures the current exception in a
  `std::exception_ptr` (function)

### External links

  1. | GOTW issue 47: Uncaught Exceptions
  2. | Rationale for `std::uncaught_exceptions` (N4125)

---
*Source: https://en.cppreference.com/w/cpp/error/uncaught_exception*
