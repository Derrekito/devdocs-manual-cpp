# std::signal

```cpp
/*signal-handler*/* signal( int sig, /*signal-handler*/* handler );  // (1)
extern "C" using /*signal-handler*/ = void(int);  // (2) (exposition only*)
```

Changes handling of the signal `sig`. Depending on `handler`, the signal can be
ignored, set to default, or handled by a user-defined function.

When signal handler is set to a function and a signal occurs, it is
implementation defined whether `std::signal(sig, SIG_DFL)` will be executed
immediately before the start of signal handler. Also, the implementation can
prevent some implementation-defined set of signals from occurring while the
signal handler runs.

For some of the signals, the implementation may call `std::signal(sig, SIG_IGN)`
at the startup of the program. For the rest, the implementation must call
`std::signal(sig, SIG_DFL)`.

(Note: POSIX introduced `sigaction` to standardize these implementation-defined
behaviors)

### Parameters

- **sig** — the signal to set the signal handler to. It can be an
  implementation-defined value or one of the following values:
  SIGABRTSIGFPESIGILLSIGINTSIGSEGVSIGTERM defines signal types (macro constant)
- **SIGABRTSIGFPESIGILLSIGINTSIGSEGVSIGTERM** — defines signal types (macro
  constant)
- **handler** — the signal handler. This must be one of the following: `SIG_DFL`
  macro. The signal handler is set to default signal handler. `SIG_IGN` macro.
  The signal is ignored. pointer to a function. The signature of the function
  must be equivalent to the following: extern "C" void fun(int sig);
- **extern "C" void fun(int sig);**

### Return value

Previous signal handler on success or `SIG_ERR` on failure (setting a signal
handler can be disabled on some implementations).

### Signal handler

The following limitations are imposed on the user-defined function that is
installed as a signal handler.

If the signal handler is called NOT as a result of `std::abort` or `std::raise`
(asynchronous signal), the behavior is undefined if
- the signal handler calls any function within the standard library, except
- the signal handler refers to any object with static storage duration that is
  not `std::atomic` or (since C++11)volatile std::sig_atomic_t.
*(until C++17)*

A *plain lock-free atomic operation* is an invocation of a function `f` from
`<atomic>` or `<stdatomic.h>`(since C++23), such that:
- `f` is the function `std::atomic_is_lock_free`,
- `f` is the member function `is_lock_free` (e.g.
  `std::atomic::is_lock_free()`),
- `f` is a non-static member function of `std::atomic_flag`,
- `f` is a non-member function, and the first parameter of `f` has type *cv*
  std::atomic_flag*,
- `f` is a non-static member function invoked on an object `obj`, such that
  `obj.is_lock_free()` yields `true`, or
- `f` is a non-member function, and for every pointer-to-atomic argument `arg`
  passed to `f`, `std::atomic_is_lock_free(arg)` yields `true`.
The behavior is undefined if any signal handler performs any of the following:
- call to any library function, except for plain lock-free atomic operations and
  the following *signal-safe* functions (note, in particular, dynamic allocation
  is not signal-safe):
- access to an object with thread storage duration
- a `dynamic_cast` expression
- a `throw` expression
- entry to a `try` block, including function-`try`-block
- initialization of a static variable that performs dynamic non-local
  initialization (including delayed until first ODR-use)
- waits for completion of initialization of any variable with static storage
  duration due to another thread concurrently initializing it
*(since C++17)*

If the user defined function returns when handling `SIGFPE`, `SIGILL`, `SIGSEGV`
or any other implementation-defined signal specifying a computational exception,
the behavior is undefined.

If the signal handler is called as a result of `std::abort` or `std::raise`
(synchronous signal), the behavior is undefined if the signal handler calls
`std::raise`.

On entry to the signal handler, the state of the floating-point environment and
the values of all objects is unspecified, except for
- objects of type volatile std::sig_atomic_t
- objects of lock-free `std::atomic` types
- side effects made visible through `std::atomic_signal_fence`
*(since C++11)*
On return from a signal handler, the value of any object modified by the signal
handler that is not volatile std::sig_atomic_t or lock-free `std::atomic` is
indeterminate.
*(until C++14)*

- objects of lock-free `std::atomic` types
- side effects made visible through `std::atomic_signal_fence`
*(since C++11)*

A call to the function `signal()` synchronizes-with any resulting invocation of
the signal handler.
If a signal handler is executed as a result of a call to `std::raise`
(synchronously), then the execution of the handler is *sequenced-after* the
invocation of `std::raise` and *sequenced-before* the return from it and runs on
the same thread as `std::raise`. Execution of the handlers for other signals is
*unsequenced* with respect to the rest of the program and runs on an unspecified
thread.
Two accesses to the same object of type volatile std::sig_atomic_t do not result
in a data race if both occur in the same thread, even if one or more occurs in a
signal handler. For each signal handler invocation, evaluations performed by the
thread invoking a signal handler can be divided into two groups A and B, such
that no evaluations in B *happen-before* evaluations in A, and the evaluations
of such volatile std::sig_atomic_t objects take values as though all evaluations
in A happened-before the execution of the signal handler and the execution of
the signal handler *happened-before* all evaluations in B.
*(since C++14)*

### Notes

POSIX requires that `signal` is thread-safe, and specifies a list of
async-signal-safe library functions that may be called from any signal handler.

Signal handlers are expected to have C linkage and, in general, only use the
features from the common subset of C and C++. However, common implementations
allow a function with C++ linkage to be used as a signal handler.

### Example

```cpp
#include <csignal>
#include <iostream>

namespace
{
    volatile std::sig_atomic_t gSignalStatus;
}

void signal_handler(int signal)
{
    gSignalStatus = signal;
}

int main()
{
    // Install a signal handler
    std::signal(SIGINT, signal_handler);

    std::cout << "SignalValue: " << gSignalStatus << '\n';
    std::cout << "Sending signal: " << SIGINT << '\n';
    std::raise(SIGINT);
    std::cout << "SignalValue: " << gSignalStatus << '\n';
}
```

Possible output:

```text
SignalValue: 0
Sending signal: 2
SignalValue: 2
```

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):
- C++17 standard (ISO/IEC 14882:2017):

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3756 | C++17 | it was unclear whether `std::atomic_flag` is signal-safe |
      it is

### See also

- **raise** — runs the signal handler for particular signal (function)
- **atomic_signal_fence (C++11)** — fence between a thread and a signal handler
  executed in the same thread (function)

**C documentation for `signal`**

---
*Source: https://en.cppreference.com/w/cpp/utility/program/signal*
