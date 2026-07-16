# std::ios_base::~ios_base

```cpp
virtual ~ios_base();
```

Destroys the `ios_base` object.

Before any of the member functions would yield undefined results, calls
callbacks, registered by `register_callback()` passing `erase_event` as
parameter. Then, deallocates any memory obtained.

No operations on `rdbuf` are performed, it is not destroyed.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3434 | C++98 | the destructor was not required to reclaim memory for
      `iarray` and `parray` | required

---
*Source: https://en.cppreference.com/w/cpp/io/ios_base/%7Eios_base*
