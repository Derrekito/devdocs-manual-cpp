# std::ostream_iterator<T,CharT,Traits>::ostream_iterator

```cpp
ostream_iterator( ostream_type& stream, const CharT* delim );  // (1)
ostream_iterator( ostream_type& stream );  // (2)
```

Constructs the iterator with `stream` as the associated stream, by storing the
address of `stream`.

1) Uses `delim` as the delimiter.

2) Uses a null pointer as the delimiter.

### Parameters

- **stream** — the output stream to be accessed by this iterator
- **delim** — the null-terminated character string to be inserted into the
  stream after each output

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <numeric>

int main()
{
    std::ostream_iterator<char> oo{std::cout};
    std::ostream_iterator<int> i1{std::cout, ", "};
    std::fill_n(i1, 5, -1);
    *oo++ = '\n';

    std::ostream_iterator<double> i2{std::cout, "; "};
    *i2++ = 3.14;
    *i2++ = 2.71;
    *oo++ = '\n';

    std::common_iterator<std::counted_iterator<std::ostream_iterator<float>>,
                         std::default_sentinel_t>
        first{std::counted_iterator{std::ostream_iterator<float>{std::cout," ~ "}, 5}},
        last{std::default_sentinel};
    std::iota(first, last, 2.2);
    *oo++ = '\n';
}
```

Output:

```text
-1, -1, -1, -1, -1,
3.14; 2.71;
2.2 ~ 3.2 ~ 4.2 ~ 5.2 ~ 6.2 ~
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 1280 | C++98 | `stream` was stored directly | stores its address instead
  P2325R3 | C++20 | default constructor was provided as C++20 iterators must be
      `default_initializable` | removed along with the requirement

---
*Source: https://en.cppreference.com/w/cpp/iterator/ostream_iterator/ostream_iterator*
