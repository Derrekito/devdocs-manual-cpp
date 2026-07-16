# std::jthread::~jthread

```cpp
~jthread();  // (since C++20)
```

Destroys the `jthread` object.

If `*this` has an associated thread (`joinable() == true`), calls
`request_stop()` and then `join()`.

### Notes

The `request_stop()` has no effect if the `jthread` was previously requested to
stop.

A `jthread` object does not have an associated thread after

- it was default-constructed.
- it was moved from.
- `join()` has been called.
- `detach()` has been called.

If `join()` throws an exception (e.g. because deadlock is detected),
`std::terminate()` may be called.

### Example

### See also

- **(destructor)** — destructs the thread object, underlying thread must be
  joined or detached (public member function of `std::thread`)

---
*Source: https://en.cppreference.com/w/cpp/thread/jthread/%7Ejthread*
