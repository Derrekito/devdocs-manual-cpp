# iter_move(std::counted_iterator)

```cpp
friend constexpr std::iter_rvalue_reference_t<I>
    iter_move( const std::counted_iterator& i )
        noexcept(noexcept(ranges::iter_move(i.base())))
            requires std::input_iterator<I>;  // (since C++20)
```

Casts the result of dereferencing the underlying iterator to its associated
rvalue reference type. The behavior is undefined if `i.count()` is equal to
`​0​`.

The function body is equivalent to `return ranges::iter_move(i.base());`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `std::counted_iterator<I>`
is an associated class of the arguments.

### Parameters

- **i** — a source iterator adaptor

### Return value

An rvalue reference or a prvalue temporary.

### Complexity

Constant.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <iterator>
#include <string>
#include <vector>

void print(auto const& rem, auto const& v)
{
    std::cout << rem << '[' << size(v) << "] {";
    for (char comma[]{0, ' ', 0}; auto const& s : v)
        std::cout << comma << std::quoted(s), *comma = ',';
    std::cout << "}\n";
}

int main()
{
    std::vector<std::string> p{"Alpha", "Bravo", "Charlie"}, q;

    print("p", p), print("q", q);

    using RI = std::counted_iterator<std::vector<std::string>::iterator>;

    for (RI iter{p.begin(), 2}; iter != std::default_sentinel; ++iter)
        q.emplace_back(/* ADL */ iter_move(iter));

    print("p", p), print("q", q);
}
```

Possible output:

```text
p[3] {"Alpha", "Bravo", "Charlie"}
q[0] {}
p[3] {"", "", "Charlie"}
q[2] {"Alpha", "Bravo"}
```

### See also

- **iter_move (C++20)** — casts the result of dereferencing an object to its
  associated rvalue reference type (customization point object)
- **iter_swap (C++20)** — swaps the objects pointed to by two underlying
  iterators (function template)
- **move (C++11)** — obtains an rvalue reference (function template)
- **move_if_noexcept (C++11)** — obtains an rvalue reference if the move
  constructor does not throw (function template)
- **forward (C++11)** — forwards a function argument (function template)
- **ranges::move (C++20)** — moves a range of elements to a new location
  (niebloid)
- **ranges::move_backward (C++20)** — moves a range of elements to a new
  location in backwards order (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/iterator/counted_iterator/iter_move*
