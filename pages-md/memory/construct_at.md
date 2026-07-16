# std::construct_at

```cpp
template< class T, class... Args >
constexpr T* construct_at( T* p, Args&&... args );  // (since C++20)
```

Creates a `T` object initialized with arguments `args...` at given address `p`.
Specialization of this function template participates in overload resolution
only if `::new(std::declval<void*>()) T(std::declval<Args>()...)` is well-formed
in an unevaluated context.

Equivalent to

```cpp
return ::new (static_cast<void*>(p)) T(std::forward<Args>(args)...);
```

except that `construct_at` may be used in evaluation of constant expressions.

When `construct_at` is called in the evaluation of some constant expression `e`,
the argument `p` must point to either storage obtained by
`std::allocator<T>::allocate` or an object whose lifetime began within the
evaluation of `e`.

### Parameters

- **p** — pointer to the uninitialized storage on which a `T` object will be
  constructed
- **args...** — arguments used for initialization

### Return value

`p`

### Example

```cpp
#include <iostream>
#include <memory>

struct S
{
    int x;
    float y;
    double z;

    S(int x, float y, double z) : x{x}, y{y}, z{z} { std::cout << "S::S();\n"; }

    ~S() { std::cout << "S::~S();\n"; }

    void print() const
    {
        std::cout << "S { x=" << x << "; y=" << y << "; z=" << z << "; };\n";
    }
};

int main()
{
    alignas(S) unsigned char storage[sizeof(S)];

    S* ptr = std::construct_at(reinterpret_cast<S*>(storage), 42, 2.71828f, 3.1415);
    ptr->print();

    std::destroy_at(ptr);
}
```

Output:

```text
S::S();
S { x=42; y=2.71828; z=3.1415; };
S::~S();
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3870 | C++20 | `construct_at` could create objects of a cv-qualified types
      | only cv-unqualified types are permitted

### See also

- **allocate** — allocates uninitialized storage (public member function of
  `std::allocator<T>`)
- **construct [static]** — constructs an object in the allocated storage
  (function template)
- **destroy_at (C++17)** — destroys an object at a given address (function
  template)
- **ranges::construct_at (C++20)** — creates an object at a given address
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/memory/construct_at*
