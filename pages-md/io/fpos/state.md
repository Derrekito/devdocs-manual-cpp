# std::fpos<State>::state

```cpp
State state() const;  // (1)
void state( State st );  // (2)
```

Manages the file position state.

1) Returns the value of the file position state.

2) Replaces the file position state with the value of `st`.

For the specializations of `std::fpos` that are used in the standard library,
`State` is always `std::mbstate_t`.

### Parameters

- **st** — new value for the state

### Return value

1) The current value of the `fpos` state.

2) (none)

### Example

```cpp
#include <cwchar>
#include <iostream>
#include <sstream>

int main()
{
    std::istringstream s("test");
    std::mbstate_t st = s.tellg().state();

    if (std::mbsinit(&st))
        std::cout << "The stream is in the initial shift state\n";
}
```

Output:

```text
The stream is in the initial shift state
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 441 | C++98 | overload (1) was not declared const (it is const in the
      synopsis) | added const

### See also

- **mbstate_t** — conversion state information necessary to iterate multibyte
  character strings (class)

---
*Source: https://en.cppreference.com/w/cpp/io/fpos/state*
