# std::auto_ptr

```cpp
template< class T > class auto_ptr;  // (1) (deprecated in C++11) (removed in C++17)
template<> class auto_ptr<void>;  // (2) (deprecated in C++11) (removed in C++17)
```

`auto_ptr` is a smart pointer that manages an object obtained via new expression
and deletes that object when `auto_ptr` itself is destroyed. It may be used to
provide exception safety for dynamically allocated objects, for passing
ownership of dynamically allocated objects into functions and for returning
dynamically allocated objects from functions.

Copying an `auto_ptr` copies the pointer and transfers ownership to the
destination: both copy construction and copy assignment of `auto_ptr` modify
their right-hand arguments, and the "copy" is not equal to the original. Because
of these unusual copy semantics, `auto_ptr` may not be placed in standard
containers. `std::unique_ptr` is preferred for this and other uses.(since C++11)

2) Specialization for type `void` is provided, it declares the typedef
   `element_type`, but no member functions.

An additional class template `auto_ptr_ref` is referred to throughout the
documentation. It is an implementation-defined type that holds a reference to
`auto_ptr`. The implementation is allowed to provide the template with a
different name or implement the functions returning it or accepting it as
parameter in other ways.

### Member types

- **`element_type`** — `T`

### Member functions

- **(constructor)** — creates a new `auto_ptr` (public member function)
- **(destructor)** — destroys an `auto_ptr` and the managed object (public
  member function)
- **operator=** — transfers ownership from another `auto_ptr` (public member
  function)
- **operator auto_ptr<Y>operator auto_ptr_ref<Y>** — converts the managed
  pointer to a pointer to different type (public member function)

**Observers**

- **get** — returns a pointer to the managed object (public member function)
- **operator*operator->** — accesses the managed object (public member function)

**Modifiers**

- **reset** — replaces the managed object (public member function)
- **release** — releases ownership of the managed object (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/memory/auto_ptr*
