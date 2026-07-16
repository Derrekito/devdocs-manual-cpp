# swap(std::move_only_function)

```cpp
friend void swap( move_only_function& lhs, move_only_function& rhs ) noexcept;  // (since C++23)
```

Overloads the `std::swap` algorithm for `std::move_only_function`. Exchanges the
state of `lhs` with that of `rhs`. Effectively calls `lhs.swap(rhs)`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::move_only_function<FunctionType>` is an associated class of the arguments.

### Parameters

- **lhs, rhs** — `std::move_only_function` objects whose states to swap

### Return value

(none)

### Example

```cpp
#include <concepts>
#include <functional>
#include <iostream>

void foo(const char* str, int x)
{
    std::cout << "foo(\"" << str << "\", " << x << ")\n";
}

void bar(const char* str, int x)
{
    std::cout << "bar(\"" << str << "\", " << x << ")\n";
}

int main()
{
    std::move_only_function<void(const char*, int) const> f1{foo};
    std::move_only_function<void(const char*, int) const> f2{bar};

    f1("f1", 1);
    f2("f2", 2);

    std::cout << "std::ranges::swap(f1, f2);\n";
    std::ranges::swap(f1, f2); // finds the hidden friend

    f1("f1", 1);
    f2("f2", 2);
}
```

Output:

```text
foo("f1", 1)
bar("f2", 2)
std::ranges::swap(f1, f2);
bar("f1", 1)
foo("f2", 2)
```

### See also

- **swap (C++23)** — swaps the targets of two `std::move_only_function` objects
  (public member function)
- **std::swap(std::function) (C++11)** — specializes the `std::swap` algorithm
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/move_only_function/swap2*
