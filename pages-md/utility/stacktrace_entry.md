# std::stacktrace_entry

```cpp
class stacktrace_entry;  // (since C++23)
```

The `stacktrace_entry` class provides operations for querying information about
an evaluation in a stacktrace. Each `stacktrace_entry` object is either empty,
or represents an evaluation in a stacktrace.

`stacktrace_entry` models `std::regular` and
`std::three_way_comparable<std::strong_ordering>`.

### Member types

- **`native_handle_type`(C++23)** — implementation-defined native handle type

### Member functions

- **(constructor) (C++23)** — constructs a new `stacktrace_entry` (public member
  function)
- **(destructor) (C++23)** — destroys the `stacktrace_entry` (public member
  function)
- **operator= (C++23)** — assigns the contents of one `stacktrace_entry` to
  another (public member function)

**Observers**

- **native_handle (C++23)** — gets the implementation-defined native handle of
  the `stacktrace_entry` (public member function)
- **operator bool (C++23)** — checks whether the `stacktrace_entry` is empty
  (public member function)

**Query**

- **description (C++23)** — gets the description of the evaluation represented
  by the `stacktrace_entry` (public member function)
- **source_file (C++23)** — gets the name of the source file that lexically
  contains the expression or statement whose evaluation is represented by the
  `stacktrace_entry` (public member function)
- **source_line (C++23)** — gets the line number that lexically relates the
  evaluation represented by the `stacktrace_entry` (public member function)

### Non-member functions

- **operator==operator<=> (C++23)** — compares two `stacktrace_entry` values
  (function)
- **to_string (C++23)** — returns a string with a description of the
  `stacktrace_entry` (function)
- **operator<< (C++23)** — performs stream output of `stacktrace_entry`
  (function template)

### Helper classes

- **std::hash<std::stacktrace_entry> (C++23)** — hash support for
  **`std::stacktrace_entry`** (class template specialization)
- **std::formatter<std::stacktrace_entry> (C++23)** — formatting support for
  `stacktrace_entry` (class template specialization)

### Notes

`boost::stacktrace::frame` (available in Boost.Stacktrace) can be used instead
when `std::stacktrace_entry` is not available.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_stacktrace` | 202011L | (C++23) | Stacktrace library

### Example

### See also

- **basic_stacktrace (C++23)** — approximate representation of an invocation
  sequence consists of stacktrace entries (class template)
- **source_location (C++20)** — a class representing information about the
  source code, such as file names, line numbers, and function names (class)

---
*Source: https://en.cppreference.com/w/cpp/utility/stacktrace_entry*
