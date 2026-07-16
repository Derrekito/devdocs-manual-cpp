# std::swap(std::shared_ptr)

```cpp
template< class T >
void swap( std::shared_ptr<T>& lhs, std::shared_ptr<T>& rhs ) noexcept;  // (since C++11)
```

Specializes the `std::swap` algorithm for `std::shared_ptr`. Swaps the contents
of `lhs` and `rhs`. Calls `lhs.swap(rhs)`.

### Parameters

- **lhs, rhs** — smart pointers whose contents to swap

### Return value

(none)

### Complexity

Constant.

### Example

```cpp
#include <iostream>
#include <memory>
#include <string>

struct Foo {
    Foo(int _val) : val(_val) { std::cout << "Foo...\n"; }
    ~Foo() { std::cout << "~Foo...\n"; }
    std::string print() { return std::to_string(val); }
    int val;
};

int main()
{
    std::shared_ptr<Foo> p1 = std::make_shared<Foo>(100);
    std::shared_ptr<Foo> p2 = std::make_shared<Foo>(200);
    auto print = [&]() {
        std::cout << " p1=" << (p1 ? p1->print() : "nullptr");
        std::cout << " p2=" << (p2 ? p2->print() : "nullptr") << '\n';
    };
    print();

    std::swap(p1, p2);
    print();

    p1.reset();
    print();

    std::swap(p1, p2);
    print();
}
```

Output:

```text
Foo...
Foo...
 p1=100 p2=200
 p1=200 p2=100
~Foo...
 p1=nullptr p2=100
 p1=100 p2=nullptr
~Foo...
```

### See also

- **swap** — swaps the values of two objects (function template)
- **swap** — swaps the contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/shared_ptr/swap2*
