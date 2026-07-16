# std::terminate

```cpp
void terminate();  // (until C++11)
[[noreturn]] void terminate() noexcept;  // (since C++11)
```

`std::terminate()` is called by the C++ runtime when the program cannot continue
for any of the following reasons:

1) an exception is thrown and not caught (it is implementation-defined whether
   any stack unwinding is done in this case)

2) a function directly invoked by the exception handling mechanism while
   handling an exception that has not yet been caught exits via an exception
   (e.g. a destructor of some local object, or a copy constructor constructing a
   catch-clause parameter)

3) the constructor or the destructor of a static or thread-local(since C++11)
   object throws an exception

4) a function registered with `std::atexit` or `std::at_quick_exit`(since C++11)
   throws an exception

5) a dynamic exception specification is violated and the default handler for
`std::unexpected` is executed
6) a non-default handler for `std::unexpected` throws an exception that violates
the previously violated dynamic exception specification, if the specification
does not include `std::bad_exception`
*(until C++17)*

7) a noexcept specification is violated (it is implementation-defined whether
any stack unwinding is done in this case)
8) `std::nested_exception::rethrow_nested` is called for an object that isn't
holding a captured exception
9) an exception is thrown from the initial function of `std::thread`
10) a joinable `std::thread` is destroyed or assigned to
11) `std::condition_variable::wait`, `std::condition_variable::wait_until`, or
`std::condition_variable::wait_for` fails to reach its postcondition (e.g. if
relocking the mutex throws)
*(since C++11)*

12) a function invoked by a parallel algorithm exits via an uncaught exception
and the execution policy specifies termination.
*(since C++17)*

`std::terminate()` may also be called directly from the program.

When `std::terminate` is called due to a thrown exception, an implicit try/catch
handler is considered active. Thus, calling `std::current_exception` will return
the thrown exception.

In any case, `std::terminate` calls the currently installed
`std::terminate_handler`. The default `std::terminate_handler` calls
`std::abort`.

If a destructor reset the terminate handler during stack unwinding and the
unwinding later led to `terminate` being called, the handler that was installed
at the end of the throw expression is the one that will be called. (note: it was
ambiguous whether re-throwing applied the new handlers)
*(until C++11)*

If a destructor reset the terminate handler during stack unwinding, it is
unspecified which handler is called if the unwinding later led to `terminate`
being called.
*(since C++11)*

### Parameters

(none)

### Return value

(none)

### Notes

If the handler mechanism is not wanted, e.g. because it requires atomic
operations which may bloat binary size, a direct call to `std::abort` is
preferred when terminating the program abnormally.

Some compiler intrinsics, e.g. `__builtin_trap` (gcc, clang, and icc) or
`__debugbreak` (msvc), can be used to terminate the program as fast as possible.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2111 | C++11 | effect of calling `set_terminate` during stack unwinding
      differs from C++98 and breaks some ABIs | made unspecified

### See also

- **terminate_handler** — the type of the function called by `std::terminate`
  (typedef)
- **abort** — causes abnormal program termination (without cleaning up)
  (function)

---
*Source: https://en.cppreference.com/w/cpp/error/terminate*
