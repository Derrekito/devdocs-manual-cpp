# std::unique_ptr<T,Deleter>::release

```cpp
pointer release() noexcept;  // (since C++11) (constexpr since C++23)
```

Releases the ownership of the managed object, if any.

`get()` returns `nullptr` after the call.

The caller is responsible for cleaning up the object (e.g. by use of
`get_deleter()`).

### Parameters

(none)

### Return value

Pointer to the managed object or `nullptr` if there was no managed object, i.e.
the value which would be returned by `get()` before the call.

### Example

```cpp
#include <cassert>
#include <iostream>
#include <memory>

struct Foo
{
    Foo() { std::cout << "Foo\n"; }
    ~Foo() { std::cout << "~Foo\n"; }
};

// Ownership of the Foo resource is transferred when calling this function
void legacy_api(Foo* owning_foo)
{
    std::cout << __func__ << '\n';
    // [legacy code that no one understands or dares touch anymore]
    // [...]
    delete owning_foo;
}

int main()
{
    std::unique_ptr<Foo> managed_foo(new Foo);
    // [code that might return or throw or some such]
    // [...]
    legacy_api(managed_foo.release());

    assert(managed_foo == nullptr);
}
```

Output:

```text
Foo
legacy_api
~Foo
```

### See also

- **get** — returns a pointer to the managed object (public member function)
- **get_deleter** — returns the deleter that is used for destruction of the
  managed object (public member function)
- **reset** — replaces the managed object (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/unique_ptr/release*
