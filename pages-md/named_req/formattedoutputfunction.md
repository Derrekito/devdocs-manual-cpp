# C++ named requirements: FormattedOutputFunction

### Requirements

A FormattedOutputFunction is a stream output function that performs the
following:

- Constructs an object of type `basic_ostream::sentry` with automatic storage
  duration, which performs the following:
- Checks the status of the sentry by calling `sentry::operator bool()`, which is
  equivalent to `basic_ios::good`.
- If the sentry returned `false` or sentry's constructor threw an exception, no
  output takes place.
- If the sentry returned `true`, attempts to perform the desired output by
  inserting the characters into the output stream as if by calling
  `rdbuf()->sputc()`. Other public members of `std::basic_ostream` may also be
  used, but virtual members of `rdbuf()` except `overflow()`, `xsputn()` and
  `sync()` will never be called.
- In any event, whether terminating by exception or returning, the sentry's
  destructor is called before leaving this function.

### Padding

Formatted output functions determine padding according to
`std::num_put::do_put()` stage 3.
*(until C++14)*

If a formatted output function of a stream `os` determines padding, it does so
as follows.
Given a `CharT` character sequence `seq` where `CharT` is the character type of
`os`, if the length of `seq` is less than `os.width()`, then enough copies of
`os.fill()` are added to this sequence as necessary to pad to a width of
`os.width()` characters.
If `(os.flags() & std::ios_base::adjustfield) == std::ios_base::left` is `true`,
the fill characters are placed after the character sequence; otherwise, they are
placed before the character sequence.
*(since C++14)*

### Standard library

The following standard library functions are **FormattedOutputFunctions**.

- `basic_ostream::operator<<(std::basic_ostream&, int / long / double / void* /
  bool)`
- `operator<<(std::basic_ostream&, char)`
- `operator<<(std::basic_ostream&, char*)`
- `operator<<(std::basic_ostream&, const std::bitset&)`
- `operator<<(std::basic_ostream&, const std::basic_string&)`
- `operator<<`, when called on the return value of `std::put_money`

- `operator<<(std::basic_ostream&, std::basic_string_view)`
*(since C++17)*

- `print(std::ostream&, std::format_string<Args...>, Args&&...)`,
- `println(std::ostream&, std::format_string<Args...>, Args&&...)`,
- `vprint_unicode(std::ostream&, std::string_view, std::format_args)`,
- `vprint_nonunicode(std::ostream&, std::string_view, std::format_args)`,
except that (given `os` is an output stream object):
- any exception thrown by the call to `std::vformat` is propagated without
  regard to the value of `os.exceptions()` and without turning on
  `ios_base::badbit` in the error state of `os`;
- if inserting into `os` fails, calls `os.setstate(ios_base::badbit)` (which may
  throw `ios_base::failure`).
*(since C++23)*

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 160 | C++98 | the process of determining whether the exception caught is
      rethrown mentioned a non-existing function `exception()` | corrected to
      `exceptions()`
  LWG 165 | C++98 | the only virtual member allowed to be called on `rdbuf()`
      was `overflow()` | also allowed `xsputn()` and `sync()`

---
*Source: https://en.cppreference.com/w/cpp/named_req/FormattedOutputFunction*
