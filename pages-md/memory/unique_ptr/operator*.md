# std::unique_ptr<T,Deleter>::operator*, std::unique_ptr<T,Deleter>::operator->

```cpp
typename std::add_lvalue_reference<T>::type operator*() const
    noexcept(noexcept(*std::declval<pointer>()));  // (1) (since C++11) (constexpr since C++23)
pointer operator->() const noexcept;  // (2) (since C++11) (constexpr since C++23)
```

`operator*` and `operator->` provide access to the object owned by `*this`.

The behavior is undefined if `get() == nullptr`.

These member functions are only provided for `unique_ptr` for the single objects
i.e. the primary template.

### Parameters

(none)

### Return value

1) Returns the object owned by `*this`, equivalent to `*get()`.

2) Returns a pointer to the object owned by `*this`, i.e. `get()`.

### Exceptions

1) May throw if `pointer` has a throwing `operator*`.

### Notes

The use of `std::add_lvalue_reference` is to make it possible to instantiate
`std::unique_ptr<void>` since `void&` isn't allowed in C++ while
`std::add_lvalue_reference<void>` produces `void`. See LWG673 for details.

### Example

```cpp
#include <iostream>
#include <memory>

struct Foo
{
    void bar() { std::cout << "Foo::bar\n"; }
};

void f(const Foo&)
{
    std::cout << "f(const Foo&)\n";
}

int main()
{
    std::unique_ptr<Foo> ptr(new Foo);

    ptr->bar();
    f(*ptr);
}
```

Output:

```text
Foo::bar
f(const Foo&)
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2762 | C++11 | `operator*` might be potentially-throwing even if `*get()`
      was noexcept | a conditional exception specification added

### See also

- **get** — returns a pointer to the managed object (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/unique_ptr/operator**
