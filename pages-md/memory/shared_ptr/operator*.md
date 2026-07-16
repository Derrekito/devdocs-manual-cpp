# std::shared_ptr<T>::operator*, std::shared_ptr<T>::operator->

```cpp
T& operator*() const noexcept;  // (1) (since C++11)
T* operator->() const noexcept;  // (2) (since C++11)
```

Dereferences the stored pointer. The behavior is undefined if the stored pointer
is null.

### Parameters

(none)

### Return value

1) The result of dereferencing the stored pointer, i.e., `*get()`.

2) The stored pointer, i.e., `get()`.

### Remarks

When `T` is an array type or (possibly cv-qualified)(since C++17) `void`, it is
unspecified whether function (1) is declared. If it is declared, it is
unspecified what its return type is, except that the declaration (although not
necessarily the definition) of the function shall be well formed. This makes it
possible to instantiate `std::shared_ptr<void>`

When `T` is an array type, it is unspecified whether function (2) is declared.
If it is declared, it is unspecified what its return type is, except that the
declaration of the function shall be well-formed.
*(since C++17)*

### Example

```cpp
#include <iostream>
#include <memory>

struct Foo
{
    Foo(int in) : a(in) {}
    void print() const
    {
        std::cout << "a = " << a << '\n';
    }
    int a;
};

int main()
{
    auto ptr = std::make_shared<Foo>(10);
    ptr->print();
    (*ptr).print();
}
```

Output:

```text
a = 10
a = 10
```

### See also

- **get** — returns the stored pointer (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/shared_ptr/operator**
