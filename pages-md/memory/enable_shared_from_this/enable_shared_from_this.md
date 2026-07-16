# std::enable_shared_from_this<T>::enable_shared_from_this

```cpp
constexpr enable_shared_from_this() noexcept;  // (1) (since C++11)
enable_shared_from_this( const enable_shared_from_this& other ) noexcept;  // (2) (since C++11)
```

Constructs a new `enable_shared_from_this` object. The exposition only
`std::weak_ptr<T>` member `weak-this` is value-initialized.

### Parameters

- **other** — an `enable_shared_from_this` to copy

### Notes

There is no move constructor: moving from an object derived from
`enable_shared_from_this` does not transfer its shared identity.

### Example

```cpp
#include <memory>

struct Foo : public std::enable_shared_from_this<Foo>
{
    Foo() {} // implicitly calls enable_shared_from_this constructor
    std::shared_ptr<Foo> getFoo() { return shared_from_this(); }
};

int main()
{
    std::shared_ptr<Foo> pf1(new Foo);
    auto pf2 = pf1->getFoo(); // shares ownership of object with pf1
}
```

### See also

- **shared_ptr (C++11)** — smart pointer with shared object ownership semantics
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/memory/enable_shared_from_this/enable_shared_from_this*
