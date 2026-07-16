# std::swap(std::weak_ptr)

```cpp
template< class T >
void swap( std::weak_ptr<T>& lhs, std::weak_ptr<T>& rhs ) noexcept;  // (since C++11)
```

Specializes the `std::swap` algorithm for `std::weak_ptr`. Swaps the contents of
`lhs` and `rhs`. Calls `lhs.swap(rhs)`.

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
    std::shared_ptr<Foo> sp1 = std::make_shared<Foo>(100);
    std::shared_ptr<Foo> sp2 = std::make_shared<Foo>(200);
    std::weak_ptr<Foo> wp1 = sp1;
    std::weak_ptr<Foo> wp2 = sp2;
    auto print = [&]() {
        auto p1 = wp1.lock();
        auto p2 = wp2.lock();
        std::cout << " p1=" << (p1 ? p1->print() : "nullptr");
        std::cout << " p2=" << (p2 ? p2->print() : "nullptr") << '\n';
    };
    print();

    std::swap(wp1, wp2);
    print();

    wp1.reset();
    print();

    std::swap(wp1, wp2);
    print();
}
```

Output:

```text
Foo...
Foo...
 p1=100 p2=200
 p1=200 p2=100
 p1=nullptr p2=100
 p1=100 p2=nullptr
~Foo...
~Foo...
```

### See also

- **swap** — swaps the values of two objects (function template)
- **swap** — swaps the contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/weak_ptr/swap2*
