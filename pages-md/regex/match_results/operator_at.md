# std::match_results<BidirIt,Alloc>::operator[]

```cpp
const_reference operator[]( size_type n ) const;  // (since C++11)
```

If `n > 0` and `n < size()`, returns a reference to the `std::sub_match`
representing the part of the target sequence that was matched by the `n`th
captured marked subexpression).

If `n == 0`, returns a reference to the `std::sub_match` representing the part
of the target sequence matched by the entire matched regular expression.

if `n >= size()`, returns a reference to a `std::sub_match` representing an
unmatched sub-expression (an empty subrange of the target sequence).

The behavior is undefined unless `ready() == true`.

### Parameters

- **n** — integral number specifying which match to return

### Return value

Reference to the `std::sub_match` representing the specified matched subrange
within the target sequence.

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
    std::cout << "entire match: " << sm[0] << '\n'
              << "submatch #1: " << sm[1] << '\n';

    std::regex re2("a(a*)b");
    std::regex_search(target, sm, re2);
    std::cout << "entire match: " << sm[0] << '\n'
              << "submatch #1: " << sm[1] << '\n';
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

- **str** — returns the sequence of characters for the particular sub-match
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/regex/match_results/operator_at*
