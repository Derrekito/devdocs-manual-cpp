# std::match_results<BidirIt,Alloc>::ready

```cpp
bool ready() const;  // (since C++11)
```

Indicates if the match results are ready (valid) or not.

A default-constructed match result has no result state (is not *ready*), and can
only be made ready by one of the regex algorithms. The *ready* state implies
that all match results have been fully established.

The result of calling most member functions of the match_results object that is
not *ready* is undefined.

### Return value

`true` if the match results are ready, `false` otherwise.

### Example

```cpp
#include <iostream>
#include <regex>
#include <string>

int main()
{
    std::string target("pattern");
    std::smatch sm;
    std::cout << "default constructed smatch is "
              << (sm.ready() ? "ready\n" : "not ready\n");

    std::regex re1("tte");
    std::regex_search(target, sm, re1);

    std::cout << "after search, smatch is "
              << (sm.ready() ? "ready\n" : "not ready\n");
}
```

Output:

```text
default constructed smatch is not ready
after search, smatch is ready
```

---
*Source: https://en.cppreference.com/w/cpp/regex/match_results/ready*
