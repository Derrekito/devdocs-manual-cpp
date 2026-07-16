# std::match_results<BidirIt,Alloc>::size

```cpp
size_type size() const noexcept;  // (since C++11)
```

Returns the number of submatches, i.e. `std::distance(begin(), end())`.

Returns `​0​` if `*this` does not represent the result of a successful match.

### Parameters

(none)

### Return value

The number of submatches.

### Complexity

Constant.

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

    std::cout << sm.size() << '\n';

    std::regex_match(target, sm, re);
    std::cout << sm.size() << '\n';
}
```

Output:

```text
0
2
```

### See also

- **begincbegin** — returns iterator to the beginning of the list of sub-matches
  (public member function)
- **endcend** — returns iterator to the end of the list of sub-matches (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/regex/match_results/size*
