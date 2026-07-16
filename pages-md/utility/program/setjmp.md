# setjmp

```cpp
#define setjmp(env) /* implementation-defined */
```

Saves the current execution context into a variable `env` of type
`std::jmp_buf`. This variable can later be used to restore the current execution
context by `std::longjmp` function. That is, when a call to `std::longjmp`
function is made, the execution continues at the particular call site that
constructed the `std::jmp_buf` variable passed to `std::longjmp`. In that case
`setjmp` returns the value passed to `std::longjmp`.

The invocation of `setjmp` must appear only in one of the following contexts:

1. the entire controlling expression of `if`, `switch`, `while`, `do-while`,
   `for`.switch (setjmp(env)) { // ...
1. one operand of a relational or equality operator with the other operand an
   integer constant expression, with the resulting expression being the entire
   controlling expression of `if`, `switch`, `while`, `do-while`, `for`.if
   (setjmp(env) > 0) { // ...
1. the operand of a unary ! operator with the resulting expression being the
   entire controlling expression of `if`, `switch`, `while`, `do-while`,
   `for`.while (!setjmp(env)) { // ...
1. the entire expression of an expression statement (possibly cast to
   void).setjmp(env);

If `setjmp` appears in any other context, the behavior is undefined.

Additionally, the behavior is undefined if `setjmp` is invoked in a coroutine in
a place where the co_await operator may be used.
*(since C++20)*

Upon return to the scope of `setjmp`:

- all accessible objects, floating-point status flags, and other components of
  the abstract machine have the same values as they had when `std::longjmp` was
  executed,
- except for the non-volatile local variables in the function containing the
  invocation of `setjmp`, whose values are indeterminate if they have been
  changed since the `setjmp` invocation.

### Parameters

- **env** — variable to save the execution state of the program to

### Return value

`​0​` if the macro was called by the original code and the execution context was
saved to `env`.

Non-zero value if a non-local jump was just performed. The return value is the
same as passed to `std::longjmp`.

### Notes

Above requirements forbid using return value of `setjmp` in data flow (e.g. to
initialize or assign an object with it). The return value can only be either
used in control flow or discarded.

### Example

```cpp
#include <array>
#include <cmath>
#include <csetjmp>
#include <cstdlib>
#include <format>
#include <iostream>

std::jmp_buf solver_error_handler;

std::array<double, 2> solve_quadratic_equation(double a, double b, double c)
{
    const double discriminant = b * b - 4.0 * a * c;
    if (discriminant < 0)
        std::longjmp(solver_error_handler, true); // Go to error handler

    const double delta = std::sqrt(discriminant) / (2.0 * a);
    const double argmin = -b / (2.0 * a);
    return {argmin - delta, argmin + delta};
}

void show_quadratic_equation_solution(double a, double b, double c)
{
    std::cout << std::format("Solving {}x² + {}x + {} = 0...\n", a, b, c);
    auto [x_0, x_1] = solve_quadratic_equation(a, b, c);
    std::cout << std::format("x₁ = {}, x₂ = {}\n\n", x_0, x_1);
}

int main()
{
    if (setjmp(solver_error_handler))
    {
        // Error handler for solver
        std::cout << "No real solution\n";
        return EXIT_FAILURE;
    }

    for (auto [a, b, c] : {std::array{1, -3, 2}, {2, -3, -2}, {1, 2, 3}})
        show_quadratic_equation_solution(a, b, c);

    return EXIT_SUCCESS;
}
```

Output:

```text
Solving 1x² + -3x + 2 = 0...
x₁ = 1, x₂ = 2

Solving 2x² + -3x + -2 = 0...
x₁ = -0.5, x₂ = 2

Solving 1x² + 2x + 3 = 0...
No real solution
```

### See also

- **longjmp** — jumps to specified location (function)

**C documentation for `setjmp`**

---
*Source: https://en.cppreference.com/w/cpp/utility/program/setjmp*
