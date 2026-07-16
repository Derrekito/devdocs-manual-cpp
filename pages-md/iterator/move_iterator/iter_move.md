# iter_move(std::move_iterator)

```cpp
friend constexpr std::iter_rvalue_reference_t<Iter>
    iter_move( const std::move_iterator& i ) noexcept(/* see below */);  // (since C++20)
```

Casts the result of dereferencing the underlying iterator to its associated
rvalue reference type.

The function body is equivalent to: `return std::ranges::iter_move(i.base());`.

This function template is not visible to ordinary unqualified or qualified
lookup, and can only be found by argument-dependent lookup when
`std::move_iterator<Iter>` is an associated class of the arguments.

### Parameters

- **i** — a source move iterator

### Return value

An rvalue reference or a prvalue temporary.

### Complexity

Constant.

### Exceptions

`noexcept` specification:

`noexcept(noexcept(ranges::iter_move(i.base())))`

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <iterator>
#include <string>
#include <vector>

void print(auto const& rem, auto const& v)
{
    std::cout << rem << '[' << size(v) << "] { ";
    for (char comma[]{0, ' ', 0}; auto const& s : v)
        std::cout << comma << std::quoted(s), comma[0] = ',';
    std::cout << " }\n";
}

int main()
{
    std::vector<std::string> p{"Andromeda", "Cassiopeia", "Phoenix"}, q;

    print("p", p), print("q", q);

    using MI = std::move_iterator<std::vector<std::string>::iterator>;

    for (MI first{p.begin()}, last{p.end()}; first != last; ++first)
        q.emplace_back(/* ADL */ iter_move(first));

    print("p", p), print("q", q);
}
```

Possible output:

```text
p[3] { "Andromeda", "Cassiopeia", "Phoenix" }
q[0] {  }
p[3] { "", "", "" }
q[3] { "Andromeda", "Cassiopeia", "Phoenix" }
```

### See also

- **iter_move (C++20)** — casts the result of dereferencing an object to its
  associated rvalue reference type (customization point object)
- **iter_move (C++20)** — casts the result of dereferencing the adjusted
  underlying iterator to its associated rvalue reference type (function)
- **move (C++11)** — obtains an rvalue reference (function template)
- **move_if_noexcept (C++11)** — obtains an rvalue reference if the move
  constructor does not throw (function template)
- **forward (C++11)** — forwards a function argument (function template)
- **ranges::move (C++20)** — moves a range of elements to a new location
  (niebloid)
- **ranges::move_backward (C++20)** — moves a range of elements to a new
  location in backwards order (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/iterator/move_iterator/iter_move*
