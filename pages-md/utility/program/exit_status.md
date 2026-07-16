# EXIT_SUCCESS, EXIT_FAILURE

```cpp
#define EXIT_SUCCESS /*implementation defined*/
#define EXIT_FAILURE /*implementation defined*/
```

The `EXIT_SUCCESS` and `EXIT_FAILURE` macros expand into integral expressions
that can be used as arguments to the `std::exit` function (and, therefore, as
the values to return from the main function), and indicate program execution
status.

- **`EXIT_SUCCESS`** — successful execution of a program
- **`EXIT_FAILURE`** — unsuccessful execution of a program

### Notes

Both `EXIT_SUCCESS` and the value zero indicate successful program execution
status (see `std::exit`), although it is not required that `EXIT_SUCCESS` equals
zero.

### See also

**C documentation for `EXIT_SUCCESS, EXIT_FAILURE`**

---
*Source: https://en.cppreference.com/w/cpp/utility/program/EXIT_status*
