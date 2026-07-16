# SIGTERM, SIGSEGV, SIGINT, SIGILL, SIGABRT, SIGFPE

```cpp
#define SIGTERM /*implementation defined*/
#define SIGSEGV /*implementation defined*/
#define SIGINT /*implementation defined*/
#define SIGILL /*implementation defined*/
#define SIGABRT /*implementation defined*/
#define SIGFPE /*implementation defined*/
```

Each of the above macro constants expands to an integer constant expression with
distinct values, which represent different signals sent to the program.

- **`SIGTERM`** — termination request, sent to the program
- **`SIGSEGV`** — invalid memory access (segmentation fault)
- **`SIGINT`** — external interrupt, usually initiated by the user
- **`SIGILL`** — invalid program image, such as invalid instruction
- **`SIGABRT`** — abnormal termination condition, as is e.g. initiated by
  `std::abort()`
- **`SIGFPE`** — erroneous arithmetic operation such as divide by zero

### Notes

Additional signal names are specified by POSIX.

### See also

- **signal** — sets a signal handler for particular signal (function)
- **raise** — runs the signal handler for particular signal (function)

**C documentation for signal types**

---
*Source: https://en.cppreference.com/w/cpp/utility/program/SIG_types*
