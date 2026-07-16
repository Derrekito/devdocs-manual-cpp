# std::basic_ofstream<CharT,Traits>::is_open

```cpp
bool is_open() const;
```

Checks if the file stream has an associated file.

Effectively calls `rdbuf()->is_open()`.

### Parameters

(none)

### Return value

`true` if the file stream has an associated file, `false` otherwise.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 365 | C++98 | `is_open` was not declared with const qualifier | declared
      with const qualifier

### See also

- **open** — opens a file and associates it with the stream (public member
  function)
- **close** — closes the associated file (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ofstream/is_open*
