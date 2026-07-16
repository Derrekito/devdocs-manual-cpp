# std::unique_ptr<T,Deleter>::reset

```cpp
members of the primary template, unique_ptr<T>
void reset( pointer ptr = pointer() ) noexcept;  // (1) (constexpr since C++23)
members of the specialization unique_ptr<T[]>
template< class U >
void reset( U ptr ) noexcept;  // (2) (constexpr since C++23)
void reset( std::nullptr_t = nullptr ) noexcept;  // (3) (constexpr since C++23)
```

Replaces the managed object.

1) Given `current_ptr`, the pointer that was managed by `*this`, performs the
   following actions, in this order: Saves a copy of the current pointer
   `old_ptr = current_ptr`. Overwrites the current pointer with the argument
   `current_ptr = ptr`. If the old pointer was non-empty, deletes the previously
   managed object `if (old_ptr) get_deleter()(old_ptr)`.

2) Behaves the same as the reset member of the primary template, except that it
   will only participate in overload resolution if either `U` is the same type
   as `pointer`, or `pointer` is the same type as `element_type*` and `U` is a
   pointer type `V*` such that `V(*)[]` is convertible to `element_type(*)[]`.

3) Equivalent to `reset(pointer()).`

### Parameters

- **ptr** — pointer to a new object to manage

### Return value

(none)

### Notes

To replace the managed object while supplying a new deleter as well, move
assignment operator may be used.

A test for self-reset, i.e. whether `ptr` points to an object already managed by
`*this`, is not performed, except where provided as a compiler extension or as a
debugging assert. Note that code such as `p.reset(p.release())` does not involve
self-reset, only code like `p.reset(p.get())` does.

### Example

```cpp
#include <iostream>
#include <memory>

struct Foo // object to manage
{
    Foo() { std::cout << "Foo...\n"; }
    ~Foo() { std::cout << "~Foo...\n"; }
};

struct D // deleter
{
    void operator() (Foo* p)
    {
        std::cout << "Calling delete for Foo object... \n";
        delete p;
    }
};

int main()
{
    std::cout << "Creating new Foo...\n";
    std::unique_ptr<Foo, D> up(new Foo(), D()); // up owns the Foo pointer (deleter D)

    std::cout << "Replace owned Foo with a new Foo...\n";
    up.reset(new Foo());  // calls deleter for the old one

    std::cout << "Release and delete the owned Foo...\n";
    up.reset(nullptr);
}
```

Output:

```text
Creating new Foo...
Foo...
Replace owned Foo with a new Foo...
Foo...
Calling delete for Foo object...
~Foo...
Release and delete the owned Foo...
Calling delete for Foo object...
~Foo...
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2118 | C++11 | `unique_ptr<T[]>::reset` rejected qualification conversions
      | accepts

### See also

- **release** — returns a pointer to the managed object and releases the
  ownership (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/unique_ptr/reset*
