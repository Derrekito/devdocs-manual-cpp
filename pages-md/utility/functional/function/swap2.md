# std::swap(std::function)

```cpp
template< class R, class... Args >
void swap( std::function<R(Args...)>& lhs, std::function<R(Args...)>& rhs ) noexcept;  // (since C++11)
```

Overloads the `std::swap` algorithm for `std::function`. Exchanges the state of
`lhs` with that of `rhs`. Effectively calls `lhs.swap(rhs)`.

### Parameters

- **lhs, rhs** — polymorphic function wrappers whose states to swap

### Return value

(none)

### Example

```cpp
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
    std::function<void(const char*, int)> f1{foo};
    std::function<void(const char*, int)> f2{bar};

    f1("f1", 1);
    f2("f2", 2);

    std::cout << "std::swap(f1, f2);\n";
    std::swap(f1, f2);

    f1("f1", 1);
    f2("f2", 2);
}
```

Output:

```text
foo("f1", 1)
bar("f2", 2)
std::swap(f1, f2);
bar("f1", 1)
foo("f2", 2)
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2062 | C++11 | overload of `swap` for `function` was not required to be
      noexcept | required

### See also

- **swap** — swaps the contents (public member function)
- **swap(std::move_only_function) (C++23)** — overloads the `std::swap`
  algorithm (function)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/function/swap2*
