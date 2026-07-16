# std::optional<T>::reset

```cpp
void reset() noexcept;  // (since C++17) (until C++20)
constexpr void reset() noexcept;  // (since C++20)
```

If `*this` contains a value, destroy that value as if by `value().T::~T()`.
Otherwise, there are no effects.

`*this` does not contain a value after this call.

### Example

```cpp
#include <optional>
#include <iostream>

struct A {
    std::string s;
    A(std::string str) : s(std::move(str))  { std::cout << " constructed\n"; }
    ~A() { std::cout << " destructed\n"; }
    A(const A& o) : s(o.s) { std::cout << " copy constructed\n"; }
    A(A&& o) : s(std::move(o.s)) { std::cout << " move constructed\n"; }
    A& operator=(const A& other) {
        s = other.s;
        std::cout << " copy assigned\n";
        return *this;
    }
    A& operator=(A&& other) {
        s = std::move(other.s);
        std::cout << " move assigned\n";
        return *this;
    }
};

int main()
{
    std::cout << "Create empty optional:\n";
    std::optional<A> opt;

    std::cout << "Construct and assign value:\n";
    opt = A("Lorem ipsum dolor sit amet, consectetur adipiscing elit nec.");

    std::cout << "Reset optional:\n";
    opt.reset();
    std::cout << "End example\n";
}
```

Output:

```text
Create empty optional:
Construct and assign value:
 constructed
 move constructed
 destructed
Reset optional:
 destructed
End example
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2231R1 | C++20 | `reset` was not constexpr while non-trivial destruction is
      allowed in constexpr in C++20 | made constexpr

### See also

- **operator=** — assigns contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/optional/reset*
