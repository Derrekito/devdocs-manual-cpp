# std::enable_shared_from_this<T>::operator=

```cpp
enable_shared_from_this& operator=( const enable_shared_from_this &rhs ) noexcept;  // (since C++11)
```

Does nothing; returns `*this`.

### Parameters

- **rhs** — another `enable_shared_from_this` to assign to `*this`

### Return value

`*this`

### Notes

The exposition-only `std::weak_ptr<T>` member `weak-this` is not affected by
this assignment operator.

### Example

Note: `enable_shared_from_this::operator=` is defined as `protected` in order to
prevent accidental slicing but allow derived classes to have default assignment
operators.

```cpp
#include <iostream>
#include <memory>

class SharedInt : public std::enable_shared_from_this<SharedInt>
{
public:
    explicit SharedInt(int n) : mNumber(n) {}
    SharedInt(const SharedInt&) = default;
    SharedInt(SharedInt&&) = default;
    ~SharedInt() = default;

    // Both assignment operators use enable_shared_from_this::operator=
    SharedInt& operator=(const SharedInt&) = default;
    SharedInt& operator=(SharedInt&&) = default;

    int number() const { return mNumber; }

private:
    int mNumber;
};

int main()
{
    std::shared_ptr<SharedInt> a = std::make_shared<SharedInt>(2);
    std::shared_ptr<SharedInt> b = std::make_shared<SharedInt>(4);
    *a = *b;

    std::cout << a->number() << '\n';
}
```

Output:

```text
4
```

### See also

- **shared_ptr (C++11)** — smart pointer with shared object ownership semantics
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/memory/enable_shared_from_this/operator%3D*
