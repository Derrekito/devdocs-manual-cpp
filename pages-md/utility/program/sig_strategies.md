# SIG_DFL, SIG_IGN

```cpp
#define SIG_DFL /*implementation defined*/
#define SIG_IGN /*implementation defined*/
```

The `SIG_DFL` and `SIG_IGN` macros expand into integral expressions that are not
equal to an address of any function. The macros define signal handling
strategies for `std::signal()` function.

- **`SIG_DFL`** — default signal handling
- **`SIG_IGN`** — signal is ignored

### See also

**C documentation for `SIG_DFL, SIG_IGN`**

---
*Source: https://en.cppreference.com/w/cpp/utility/program/SIG_strategies*
