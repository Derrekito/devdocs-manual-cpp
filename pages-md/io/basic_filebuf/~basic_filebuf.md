# std::basic_filebuf<CharT,Traits>::~basic_filebuf

```cpp
virtual ~basic_filebuf();
```

Calls `close()` to close the associated file and destructs all other members of
`basic_filebuf`. If an exception occurs during the destruction of the object,
including the call to `close()`, it is caught and not rethrown.

### Parameters

(none)

### Return value

(none)

### Notes

Typically called by the destructor of `std::basic_fstream`.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 622 | C++98 | it was unclear how to handle the exception thrown during
      destruction | it is caught but not rethrown

### See also

- **(constructor)** — constructs a `basic_filebuf` object (public member
  function)
- **close** — flushes the put area buffer and closes the associated file (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_filebuf/%7Ebasic_filebuf*
