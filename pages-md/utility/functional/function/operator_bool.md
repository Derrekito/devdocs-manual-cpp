# std::function<R(Args...)>::operator bool

```cpp
explicit operator bool() const noexcept;  // (since C++11)
```

Checks whether `*this` stores a callable function target, i.e. is not empty.

### Parameters

(none)

### Return value

`true` if `*this` stores a callable function target, `false` otherwise.

### Example

```cpp
#include <functional>
#include <iostream>

void sampleFunction()
{
    std::cout << "This is the sample function!\n";
}

void checkFunc(std::function<void()> const& func)
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
    std::function<void()> f1;
    std::function<void()> f2(sampleFunction);

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

- **operator bool (C++23)** — checks if the `std::move_only_function` has a
  target (public member function of `std::move_only_function`)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/function/operator_bool*
