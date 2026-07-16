# C++ named requirements: UnformattedOutputFunction

### Requirements

A UnformattedOutputFunction is a stream output function that performs the
following:

1) Constructs an object of type `basic_ostream::sentry` with automatic storage
duration, which performs the following:

- If `eofbit` or `badbit` are set on the output stream, sets the `failbit` as
  well, and if exceptions on `failbit` are enabled in this output stream's
  exception mask (`(exceptions() & failbit) != 0`), throws `ios_base::failure`.
- Flushes the tie()'d output stream, if applicable.

2) Checks the status of the sentry by calling `sentry::operator bool()`, which
is equivalent to `basic_ios::good`.

- If the operator returns `false` or sentry's constructor throws an exception,
  no output takes place.
- If the operator returns `true`, attempts to perform the desired output by
  inserting the characters into the output stream as if by calling
  `rdbuf()->sputc()`. Other public members of `std::basic_ostream` may also be
  used, but virtual members of `rdbuf()` except `overflow()`, `xsputn()` and
  `sync()` will never be called.

3) In any event, whether terminating by exception or returning, the sentry's
   destructor is called before leaving this function.

### Standard library

The following standard library functions are **UnformattedOutputFunctions**.

- `basic_ostream::operator<<(basic_streambuf*)`
- `basic_ostream::put`
- `basic_ostream::write`
- `basic_ostream::flush`

- `basic_ostream::tellp` (except calls pubseekoff instead of output)
- `basic_ostream::seekp` (except calls pubseekoff or pubseekpos instead of
  output)
*(since C++11)*

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 63 | C++98 | the exception-handling policy was missing | added
  LWG 160 | C++98 | the process of determining whether the exception caught is
      rethrown mentioned a non-existing function `exception()` | corrected to
      `exceptions()`
  LWG 165 | C++98 | the only virtual member allowed to be called on `rdbuf()`
      was `overflow()` | also allowed `xsputn()` and `sync()`

---
*Source: https://en.cppreference.com/w/cpp/named_req/UnformattedOutputFunction*
