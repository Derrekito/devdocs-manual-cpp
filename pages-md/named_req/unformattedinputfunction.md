# C++ named requirements: UnformattedInputFunction

### Requirements

An UnformattedInputFunction is a stream input function that performs the
following:

1) Constructs an object of type `basic_istream::sentry` with automatic storage
duration and with the `noskipws` argument set to `true`, which performs the
following:

- If `eofbit` or `badbit` are set on the input stream, sets the `failbit` as
  well, and if exceptions on `failbit` are enabled in this input stream's
  exception mask (`(exceptions() & failbit) != 0`), throws `ios_base::failure`.
- Flushes the tie()'d output stream, if applicable.

2) Checks the status of the sentry by calling `sentry::operator bool()`, which
is equivalent to `basic_ios::good`.

- If the operator returns `false` or sentry's constructor throws an exception:
- If the operator returns `true`, performs the input as if by calling
  `rdbuf()->sbumpc()` or `rdbuf()->sgetc()`.
- If the end of the stream is reached (the call to `rdbuf()->sbumpc()` or
  `rdbuf()->sgetc()` returns `Traits::eof()`), sets `eofbit`. If exceptions on
  `eofbit` are enabled in this stream's exception mask (`(exceptions() & eofbit)
  != 0`), throws `ios_base::failure`.
- If an exception is thrown during input, sets `badbit` in the input stream. If
  exceptions on `badbit` are enabled in this stream's exception mask
  (`(exceptions() & badbit) != 0`), the exception is also rethrown.
- If no exception was thrown during input, sets the number of extracted
  characters (gcount) in the input stream.

3) In any event, whether terminating by exception or returning, the sentry's
   destructor is called before leaving this function.

### Standard library

The following standard library functions are **UnformattedInputFunctions**.

- `std::getline`, except that it does not modify gcount.
- `basic_istream::operator>>(basic_streambuf*)`
- `basic_istream::get`
- `basic_istream::getline`
- `basic_istream::ignore`
- `basic_istream::peek`
- `basic_istream::read`
- `basic_istream::readsome`
- `basic_istream::putback`, except that it first clears `eofbit`
- `basic_istream::unget`, except that it first clears `eofbit`
- `basic_istream::sync`, except that it does not modify gcount
- `basic_istream::tellg`, except that it does not modify gcount
- `basic_istream::seekg`, except that it first clears `eofbit` and does not
  modify gcount
- `std::ws`, except that it does not modify gcount

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 61 | C++98 | it was unclear whether throwing an exception because of
      setting `eofbit` and/or `failbit` will result in setting `badbit` |
      exceptions thrown from `basic_ios::clear` are not caught or rethrown
  LWG 160 | C++98 | the process of determining whether the exception caught is
      rethrown mentioned a non-existing function `exception()` | corrected to
      `exceptions()`
  LWG 243 | C++98 | the behavior when `sentry::operator bool()` returns `false`
      or the sentry object fails to be constructed was not specified | specified

---
*Source: https://en.cppreference.com/w/cpp/named_req/UnformattedInputFunction*
