# Standard library header <csignal>

This header was originally in the C standard library as `<signal.h>`.

This header is part of the program support library.

**Typedefs**

- **sig_atomic_t** — the integer type that can be accessed as an atomic entity
  from an asynchronous signal handler (typedef)

**Macros**

- **SIGABRTSIGFPESIGILLSIGINTSIGSEGVSIGTERM** — defines signal types (macro
  constant)
- **SIG_DFLSIG_IGN** — defines signal handling strategies (macro constant)
- **SIG_ERR** — return value of `signal` specifying that an error was
  encountered (macro constant)

**Functions**

- **signal** — sets a signal handler for particular signal (function)
- **raise** — runs the signal handler for particular signal (function)

### Synopsis

```cpp
namespace std {
  using sig_atomic_t = /*see description*/ ;
  extern "C" using /*signal-handler*/ = void(int); // exposition only
  /*signal-handler*/ * signal(int sig, /*signal-handler*/ * func);
}
#define SIG_DFL /*see description*/
#define SIG_ERR /*see description*/
#define SIG_IGN /*see description*/
#define SIGABRT /*see description*/
#define SIGFPE /*see description*/
#define SIGILL /*see description*/
#define SIGINT /*see description*/
#define SIGSEGV /*see description*/
#define SIGTERM /*see description*/
```

---
*Source: https://en.cppreference.com/w/cpp/header/csignal*
