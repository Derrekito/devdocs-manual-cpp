# std::match_results<BidirIt,Alloc>::position

```cpp
difference_type position( size_type n = 0 ) const;  // (since C++11)
```

Returns the position of the first character of the specified sub-match.

If `n == 0`, the position of the first character of the entire matched
expression is returned.

If `n > 0 && n < size()`, the position of the first character of the `n`th
sub-match is returned.

if `n >= size()`, a position of the first character of the unmatched match is
returned.

### Parameters

- **n** — integral number specifying which match to examine

### Return value

The position of the first character of the specified match or sub-match.

### Example

```cpp
#include <iostream>
#include <regex>
#include <string>

int main()
{
    std::regex re("a(a)*b");
    std::string target("aaab");
    std::smatch sm;

    std::regex_match(target, sm, re);
    std::cout << sm.position(1) << '\n';
}
```

Output:

```text
2
```

### See also

- **operator[]** — returns specified sub-match (public member function)

---
*Source: https://en.cppreference.com/w/cpp/regex/match_results/position*
