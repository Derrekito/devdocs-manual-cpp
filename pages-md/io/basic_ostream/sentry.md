# std::basic_ostream<CharT,Traits>::sentry

```cpp
class sentry;
```

An object of class `basic_ostream::sentry` is constructed in local scope at the
beginning of each member function of `std::basic_ostream` that performs output
(both formatted and unformatted). Its constructor prepares the output stream:
checks if the stream is already in a failed state, flushes the tie()'d output
streams, and performs other implementation-defined tasks if necessary.
Implementation-defined cleanup, as well as flushing of the output stream if
necessary, is performed in the destructor, so that it is guaranteed to happen if
exceptions are thrown during output.

### Member functions

- ****(constructor)**** — constructs the sentry object. All the preparation
  tasks are done here (public member function)
- ****(destructor)**** — finalizes the stream object after formatted output or
  after exception, if necessary (public member function)
- **operator=** — the assignment operator is deleted (public member function)
- ****operator bool**** — checks if the preparation of the stream object was
  successful (public member function)

## std::basic_ostream::sentry::sentry

```cpp
explicit sentry( std::basic_ostream<CharT, Traits>& os );
```

Prepares the stream for formatted output.

If `os.good()` is `false`, returns. Otherwise, if `os.tie()` is not a null
pointer, calls `os.tie()->flush()` to synchronize the output sequence with
external streams. During preparation, the constructor may call
`setstate(failbit)` (which may throw `std::ios_base::failure`).

If after preparation is completed, `os.good() == true`, then any subsequent
calls to operator bool will return `true`.

### Parameters

- **os** — output stream to prepare

### Exceptions

`std::ios_base::failure` if the end of file condition occurs.

## std::basic_ostream::sentry::~sentry

```cpp
~sentry();
```

If `(os.flags() & std::ios_base::unitbuf) && !std::uncaught_exception() &&
os.good())` is `true`, calls `os.rdbuf()->pubsync()`. If that function returns
`-1`, sets `badbit` in `os.rdstate()` without propagating an exception.

## std::basic_ostream::sentry::operator bool

```cpp
explicit operator bool() const;
```

Checks whether the preparation of the output stream was successful.

### Parameters

(none)

### Return value

`true` if the preparation of the output stream was successful, `false`
otherwise.

### Example

```cpp
#include <iostream>
#include <sstream>

struct Foo
{
    char n[6];
};

std::ostream& operator<<(std::ostream& os, Foo& f)
{
    std::ostream::sentry s(os);
    if (s)
        os.write(f.n, 5);
    return os;
}

int main()
{
    Foo f = {"abcde"};
    std::cout << f << '\n';
}
```

Output:

```text
abcde
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 397 | C++98 | the destructor might call `os.flush()`, which may throw
      exceptions | the exception is not propagated
  LWG 442 | C++98 | operator bool was not declared const (it is const in the
      synopsis) | added const
  LWG 835 | C++98 | if `os` sets `unitbuf`, the destructor would call
      `os.flush()`, which is an UnformattedOutputFunction and creates another
      sentry object (whose destructor then creates another sentry object and so
      on) | calls `os.rdbuf()->pubsync()` in this case instead

### See also

- **operator<<** — inserts formatted data (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ostream/sentry*
