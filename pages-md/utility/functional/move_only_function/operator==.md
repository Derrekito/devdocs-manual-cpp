# operator==(std::move_only_function)

```cpp
friend bool operator==( const move_only_function& f, std::nullptr_t ) noexcept;  // (since C++23)
```

Checks whether the wrapper `f` has a callable target by formally comparing it
with `std::nullptr`. Empty wrappers (that is, wrappers without a target) compare
equal, non-empty functions compare non-equal.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::move_only_function<FunctionType>` is an associated class of the arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **f** — `std::move_only_function` to compare

### Return value

`!f`.

### Example

```cpp
#include <functional>
#include <iostream>
#include <utility>

using SomeVoidFunc = std::move_only_function<void(int) const>;

class C {
public:
    C() = default;
    C(SomeVoidFunc func) : void_func_(std::move(func)) {}

    void default_func(int i) const { std::cout << i << '\n'; };

    void operator()() const
    {
        if (void_func_ == nullptr) // specialized compare with nullptr
            default_func(7);
        else
            void_func_(7);
    }

private:
    SomeVoidFunc void_func_{};
};

void user_func(int i)
{
    std::cout << (i + 1) << '\n';
}

int main()
{
    C c1;
    C c2(user_func);
    c1();
    c2();
}
```

Output:

```text
7
8
```

### See also

- **operator bool (C++23)** — checks if the `std::move_only_function` has a
  target (public member function)
- **operator==operator!= (removed in C++20)** — compares a `std::function` with
  `nullptr` (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/move_only_function/operator%3D%3D*
