# std::basic_ostream<CharT,Traits>::flush

```cpp
basic_ostream& flush();
```

Writes uncommitted changes to the underlying output sequence. Behaves as an
UnformattedOutputFunction.

If `rdbuf()` is a null pointer, the sentry object is not constructed.

Otherwise, after constructing and checking the sentry object, calls
`rdbuf()->pubsync()`. If the call returns `-1`, calls `setstate(badbit)`.

### Parameters

(none)

### Return value

`*this`

### Exceptions

May throw `std::ios_base::failure` if `(exceptions() & badbit) != 0`.

### Example

```cpp
#include <chrono>
#include <iostream>
#include <thread>

using namespace std::chrono_literals;

void f()
{
    std::cout << "Output from thread... ";
    for (int i{1}; i != 10; ++i)
    {
        std::this_thread::sleep_for(250ms);
        std::cout << i << ' ';

        // output three numbers at once;
        // the effect is observable only in real-time
        if (0 == (i % 3))
            std::cout.flush();
    }
    std::cout << std::endl; // flushes as well
}

int main()
{
    std::thread tr{f};
    std::this_thread::sleep_for(150ms);
    std::clog << "Output from main\n";
    tr.join();
}
```

Output:

```text
Output from main
Output from thread... 1 2 3 4 5 6 7 8 9
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 581 | C++98 | `flush()` did not behave as an UnformattedOutputFunction
      because of the resolution of LWG issue 60 | behaves as an
      UnformattedOutputFunction

### See also

- **pubsync** — invokes `sync()` (public member function of
  `std::basic_streambuf<CharT,Traits>`)
- **sync [virtual]** — synchronizes the buffers with the associated character
  sequence (virtual protected member function of
  `std::basic_streambuf<CharT,Traits>`)
- **flush** — flushes the output stream (function template)
- **endl** — outputs `'\n'` and flushes the output stream (function template)
- **sync** — synchronizes with the underlying storage device (public member
  function of `std::basic_istream<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ostream/flush*
