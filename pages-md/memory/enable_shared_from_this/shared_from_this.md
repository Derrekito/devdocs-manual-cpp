# std::enable_shared_from_this<T>::shared_from_this

```cpp
std::shared_ptr<T> shared_from_this();  // (1) (since C++11)
std::shared_ptr<T const> shared_from_this() const;  // (2) (since C++11)
```

Returns a `std::shared_ptr<T>` that shares ownership of `*this` with all
existing `std::shared_ptr` that refer to `*this`.

Effectively executes `std::shared_ptr<T>(weak_this)`, where `weak-this` is the
exposition-only mutable `std::weak_ptr<T>` member of `enable_shared_from_this`.

### Return value

`std::shared_ptr<T>` that shares ownership of `*this` with pre-existing
`std::shared_ptr`s.

### Exceptions

If `shared_from_this` is called on an object that is not previously shared by
`std::shared_ptr`, `std::bad_weak_ptr` is thrown (by the `shared_ptr`
constructor from a default-constructed `weak-this`).

### Example

```cpp
#include <iostream>
#include <memory>

struct Foo : public std::enable_shared_from_this<Foo>
{
    Foo() { std::cout << "Foo::Foo\n"; }
    ~Foo() { std::cout << "Foo::~Foo\n"; }
    std::shared_ptr<Foo> getFoo() { return shared_from_this(); }
};

int main()
{
    Foo *f = new Foo;
    std::shared_ptr<Foo> pf1;

    {
        std::shared_ptr<Foo> pf2(f);
        pf1 = pf2->getFoo(); // shares ownership of object with pf2
    }

    std::cout << "pf2 is gone\n";
}
```

Output:

```text
Foo::Foo
pf2 is gone
Foo::~Foo
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2529 | C++11 | calling `shared_from_this` on an unshared object was
      undefined behavior | `std::bad_weak_ptr` is thrown

### See also

- **shared_ptr (C++11)** — smart pointer with shared object ownership semantics
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/memory/enable_shared_from_this/shared_from_this*
