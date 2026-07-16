# Low level memory management

The new-expression is the only way to create an object or an array of objects
with dynamic storage duration, that is, with lifetime not restricted to the
scope in which it is created. A new-expression obtains storage by calling an
allocation function. A delete-expression destroys a most derived object or an
array created by a new-expression and calls the deallocation function. The
default allocation and deallocation functions, along with related functions,
types, and objects, are declared in the header `<new>`.

**Functions**

- **operator newoperator new[]** — allocation functions (function)
- **operator deleteoperator delete[]** — deallocation functions (function)
- **get_new_handler (C++11)** — obtains the current new handler (function)
- **set_new_handler** — registers a new handler (function)

**Classes**

- **bad_alloc** — exception thrown when memory allocation fails (class)
- **bad_array_new_length (C++11)** — exception thrown on allocation of array
  with invalid length (class)
- **align_val_t (C++17)** — type used to pass alignment to alignment-aware
  allocation and deallocation functions (enum)

**Types**

- **new_handler** — function pointer type of the new handler (typedef)

**Objects**

- **nothrow** — an object of type `nothrow_t` used to select a non-throwing
  *allocation function* (constant)
- **destroying_delete (C++20)** — an object of type `std::destroying_delete_t`
  used to select destroying-delete overloads of `operator delete` (constant)

**Object access**

- **launder (C++17)** — pointer optimization barrier (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/new*
