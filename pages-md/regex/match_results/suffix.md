# std::match_results<BidirIt,Alloc>::suffix

```cpp
const_reference suffix() const;  // (since C++11)
```

Obtains a reference to the `std::sub_match` object representing the target
sequence between the end of the entire match of the regular expression and the
end of the target sequence.

The behavior is undefined unless `ready()` is `true`.

### Parameters

(none)

### Return value

Reference to the unmatched suffix.

### Example

```cpp
#include <iostream>
#include <regex>
#include <string>

int main()
{
    std::regex re("a(a)*by");
    std::string target("baaaby123");
    std::smatch sm;

    std::regex_search(target, sm, re);
    std::cout << sm.suffix() << '\n';
}
```

Output:

```text
123
```

### See also

- **prefix** — returns sub-sequence between the beginning of the target sequence
  and the beginning of the full match (public member function)

---
*Source: https://en.cppreference.com/w/cpp/regex/match_results/suffix*
