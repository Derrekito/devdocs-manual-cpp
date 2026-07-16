# deduction guides for `std::basic_regex`

```cpp
template< class ForwardIt >
basic_regex( ForwardIt, ForwardIt,
             std::regex_constants::syntax_option_type = std::regex_constants::ECMAScript )
-> basic_regex<typename std::iterator_traits<ForwardIt>::value_type>;  // (since C++17)
```

This deduction guide is provided for `std::basic_regex` to allow deduction from
an iterator range.

### Example

```cpp
#include <regex>
#include <vector>

int main()
{
    std::vector<char> v = {'a', 'b', 'c'};
    std::basic_regex re(v.begin(), v.end()); // uses explicit deduction guide
}
```

---
*Source: https://en.cppreference.com/w/cpp/regex/basic_regex/deduction_guides*
