# RAII

*Resource Acquisition Is Initialization* or RAII, is a C++ programming
technique[1][2] which binds the life cycle of a resource that must be acquired
before use (allocated heap memory, thread of execution, open socket, open file,
locked mutex, disk space, database connection—anything that exists in limited
supply) to the lifetime of an object.

RAII guarantees that the resource is available to any function that may access
the object (resource availability is a class invariant, eliminating redundant
runtime tests). It also guarantees that all resources are released when the
lifetime of their controlling object ends, in reverse order of acquisition.
Likewise, if resource acquisition fails (the constructor exits with an
exception), all resources acquired by every fully-constructed member and base
subobject are released in reverse order of initialization. This leverages the
core language features (object lifetime, scope exit, order of initialization and
stack unwinding) to eliminate resource leaks and guarantee exception safety.
Another name for this technique is *Scope-Bound Resource Management* (SBRM),
after the basic use case where the lifetime of an RAII object ends due to scope
exit.

RAII can be summarized as follows:

- encapsulate each resource into a class, where
- always use the resource via an instance of a RAII-class that either

Move semantics make it possible to safely transfer resource ownership between
objects, across scopes, and in and out of threads, while maintaining resource
safety.
*(since C++11)*

Classes with `open()`/`close()`, `lock()`/`unlock()`, or
`init()`/`copyFrom()`/`destroy()` member functions are typical examples of
non-RAII classes:

```cpp
std::mutex m;

void bad()
{
    m.lock();             // acquire the mutex
    f();                  // if f() throws an exception, the mutex is never released
    if (!everything_ok())
        return;           // early return, the mutex is never released
    m.unlock();           // if bad() reaches this statement, the mutex is released
}

void good()
{
    std::lock_guard<std::mutex> lk(m); // RAII class: mutex acquisition is initialization
    f();                               // if f() throws an exception, the mutex is released
    if (!everything_ok())
        return;                        // early return, the mutex is released
}                                      // if good() returns normally, the mutex is released
```

### The standard library

The C++ library classes that manage their own resources follow RAII:
`std::string`, `std::vector`, `std::jthread`(since C++20), and many others
acquire their resources in constructors (which throw exceptions on errors),
release them in their destructors (which never throw), and don't require
explicit cleanup.

In addition, the standard library offers several RAII wrappers to manage
user-provided resources:
- `std::unique_ptr` and `std::shared_ptr` to manage dynamically-allocated memory
  or, with a user-provided deleter, any resource represented by a plain pointer;
- `std::lock_guard`, `std::unique_lock`, `std::shared_lock` to manage mutexes.
*(since C++11)*

### Notes

RAII does not apply to the management of the resources that are not acquired
before use: CPU time, cores, and cache capacity, entropy pool capacity, network
bandwidth, electric power consumption, stack memory.

### External links

1. RAII in Stroustrup's C++ FAQ
1. C++ Core Guidelines E.6 "Use RAII to prevent leaks"

---
*Source: https://en.cppreference.com/w/cpp/language/raii*
