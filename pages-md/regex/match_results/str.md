# std::match_results<BidirIt,Alloc>::str

```cpp
string_type str( size_type n = 0 ) const;  // (since C++11)
```

Returns a string representing the indicated sub-match.

If `n == 0`, a string representing entire matched expression is returned.

If `0 < n && n < size()`, a string representing `n`th sub-match is returned.

if `n >= size()`, a string representing the unmatched match is returned.

The call is equivalent to `string_type((*this)[n])`;

### Parameters

- **n** — integral number specifying which match to return

### Return value

Returns a string representing the specified match or sub match.

### Example

```cpp
#include <iostream>
#include <regex>
#include <string>

int main()
{
    std::string target("baaaby");
    std::smatch sm;

    std::regex re1("a(a)*b");
    std::regex_search(target, sm, re1);
    std::cout << "entire match: " << sm.str(0) << '\n'
              << "submatch #1: " << sm.str(1) << '\n';

    std::regex re2("a(a*)b");
    std::regex_search(target, sm, re2);
    std::cout << "entire match: " << sm.str(0) << '\n'
              << "submatch #1: " << sm.str(1) << '\n';
}
```

Output:

```text
entire match: aaab
submatch #1: a
entire match: aaab
submatch #1: aa
```

### See also

- **operator[]** — returns specified sub-match (public member function)

---
*Source: https://en.cppreference.com/w/cpp/regex/match_results/str*
