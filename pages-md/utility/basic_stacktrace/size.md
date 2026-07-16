# std::basic_stacktrace<Allocator>::size

```cpp
size_type size() const noexcept;  // (since C++23)
```

Returns the number of entries in the stacktrace.

### Parameters

(none)

### Return value

The number of entries in the stacktrace.

### Complexity

Constant.

### Example

The following code uses `size` to display the number of entries in the current
stacktrace:

```cpp
#include <stacktrace>
#include <iostream>

int main()
{
    auto trace = std::stacktrace::current();

    std::cout << "trace contains " << trace.size() << " entries.\n";
}
```

Possible output:

```text
trace contains 3 entries.
```

### See also

- **empty** — checks whether the `basic_stacktrace` is empty (public member
  function)
- **max_size** — returns the maximum possible number of stacktrace entries
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/basic_stacktrace/size*
