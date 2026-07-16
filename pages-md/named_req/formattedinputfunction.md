# C++ named requirements: FormattedInputFunction

### Requirements

A FormattedInputFunction is a stream input function that performs the following:

- Constructs an object of type `basic_istream::sentry` with automatic storage
  duration and with the `noskipws` argument set to `false`, which performs the
  following:
- if `eofbit` or `badbit` are set on the input stream, sets the `failbit` as
  well, and if exceptions on `failbit` are enabled in this input stream's
  exception mask (`(exceptions() & failbit) != 0`), throws `ios_base::failure`.
- flushes the tie()'d output stream, if applicable.
- if `ios_base::skipws` flag is set on this input stream, extracts and discards
  characters from the input stream until one of the following becomes true:
- Checks the status of the sentry by calling `sentry::operator bool()`, which is
  equivalent to `basic_ios::good`.
- If the sentry returned `false` or sentry's constructor threw an exception, no
  input takes place.
- If the sentry returned `true`, performs the input as if by calling
  `rdbuf()->sbumpc()` or `rdbuf()->sgetc()`.
- In any event, whether terminating by exception or returning, the sentry's
  destructor is called before leaving this function.

### Standard library

The following standard library functions are **FormattedInputFunctions**.

- `basic_istream::operator>>(int, long, double, void*, bool)`
- `operator>>(std::basic_istream, char&)`
- `operator>>(std::basic_istream, char*)`
- `operator>>(std::basic_istream, std::bitset)`
- `operator>>(std::basic_istream, std::string)`
- `operator>>`, when called on the return value of `std::get_money`

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 160 | C++98 | the process of determining whether the exception caught is
      rethrown mentioned a non-existing function `exception()` | corrected to
      `exceptions()`

---
*Source: https://en.cppreference.com/w/cpp/named_req/FormattedInputFunction*
