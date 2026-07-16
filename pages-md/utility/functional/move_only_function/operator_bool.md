# std::move_only_function::operator bool

```cpp
explicit operator bool() const noexcept;  // (since C++23)
```

Checks whether `*this` stores a callable target, i.e. is not empty.

### Parameters

(none)

### Return value

`true` if `*this` stores a callable target, `false` otherwise.

### Example

```cpp
#include <functional>
#include <iostream>

void sampleFunction()
{
    std::cout << "This is the sample function!\n";
}

void checkFunc(std::move_only_function<void() const> const& func)
{
    // Use operator bool to determine if callable target is available.
    if (func)
    {
        std::cout << "Function is not empty! Calling function.\n";
        func();
    }
    else
        std::cout << "Function is empty. Nothing to do.\n";
}

int main()
{
    std::move_only_function<void() const> f1{};
    std::move_only_function<void() const> f2{sampleFunction};

    std::cout << "f1: ";
    checkFunc(f1);

    std::cout << "f2: ";
    checkFunc(f2);
}
```

Output:

```text
f1: Function is empty. Nothing to do.
f2: Function is not empty! Calling function.
This is the sample function!
```

### See also

- **operator== (C++23)** — compares a `std::move_only_function` with `nullptr`
  (function)
- **operator bool** — checks if a target is contained (public member function of
  `std::function<R(Args...)>`)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/move_only_function/operator_bool*
