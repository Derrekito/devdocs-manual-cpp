# std::stack<T,Container>::emplace

```cpp
template< class... Args >
void emplace( Args&&... args );  // (since C++11) (until C++17)
template< class... Args >
decltype(auto) emplace( Args&&... args );  // (since C++17)
```

Pushes a new element on top of the stack. The element is constructed in-place,
i.e. no copy or move operations are performed. The constructor of the element is
called with exactly the same arguments as supplied to the function.

Effectively calls `c.emplace_back(std::forward<Args>(args)...);`.

### Parameters

- **args** — arguments to forward to the constructor of the element

### Return value

(none)
*(until C++17)*

The value or reference, if any, returned by the above call to
`Container::emplace_back`.
*(since C++17)*

### Complexity

Identical to the complexity of `Container::emplace_back`.

### Example

```cpp
#include <iostream>
#include <stack>

struct S
{
    int id;

    S(int i, double d, std::string s) : id{i}
    {
        std::cout << "S::S(" << i << ", " << d << ", \"" << s << "\");\n";
    }
};

int main()
{
    std::stack<S> stack;
    const S& s = stack.emplace(42, 3.14, "C++"); // for return value C++17 required
    std::cout << "id = " << s.id << '\n';
}
```

Output:

```text
S::S(42, 3.14, "C++")
id = 42
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2783 | C++17 | `emplace` returned `reference`, breaking compatibility with
      pre-C++17 containers | returns `decltype(auto)`

### See also

- **push** — inserts element at the top (public member function)
- **pop** — removes the top element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/stack/emplace*
