# std::unique_ptr<T,Deleter>::swap

```cpp
void swap( unique_ptr& other ) noexcept;  // (since C++11)
```

Swaps the managed objects and associated deleters of `*this` and another
`unique_ptr` object `other`.

### Parameters

- **other** — another `unique_ptr` object to swap the managed object and the
  deleter with

### Return value

(none)

### Example

```cpp
#include <iostream>
#include <memory>

struct Foo
{
    Foo(int _val) : val(_val) { std::cout << "Foo...\n"; }
    ~Foo() { std::cout << "~Foo...\n"; }
    int val;
};

int main()
{
    std::unique_ptr<Foo> up1(new Foo(1));
    std::unique_ptr<Foo> up2(new Foo(2));

    up1.swap(up2);

    std::cout << "up1->val:" << up1->val << '\n';
    std::cout << "up2->val:" << up2->val << '\n';
}
```

Output:

```text
Foo...
Foo...
up1->val:2
up2->val:1
~Foo...
~Foo...
```

---
*Source: https://en.cppreference.com/w/cpp/memory/unique_ptr/swap*
