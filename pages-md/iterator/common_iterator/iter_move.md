# iter_move(std::common_iterator)

```cpp
friend std::iter_rvalue_reference_t<I>
    iter_move( const std::common_iterator& i )
        noexcept(noexcept(ranges::iter_move(std::declval<const I&>()))
            requires std::input_iterator<I>;  // (since C++20)
```

Casts the result of dereferencing the underlying iterator to its associated
rvalue reference type. The behavior is undefined if `i.var` does not hold an `I`
object (i.e. an iterator).

The function body is equivalent to: `return
std::ranges::iter_move(std::get<I>(i.var));`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `std::common_iterator<I,S>`
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
    std::cout << rem << '[' << size(v) << "] { ";
    for (int o{}; auto const& s : v)
        std::cout << (o++ ? ", " : "") << std::quoted(s);
    std::cout << " }\n";
}

int main()
{
    std::vector<std::string> p{"Andromeda", "Cassiopeia", "Phoenix"}, q;

    print("p", p), print("q", q);

    using CI = std::common_iterator<
                   std::counted_iterator<std::vector<std::string>::iterator>,
                   std::default_sentinel_t
                   >;
    CI last{std::default_sentinel};

    for (CI first{{p.begin(), 2}}; first != last; ++first)
        q.emplace_back(/* ADL */ iter_move(first));

    print("p", p), print("q", q);
}
```

Possible output:

```text
p[3] { "Andromeda", "Cassiopeia", "Phoenix" }
q[0] {  }
p[3] { "", "", "Phoenix" }
q[2] { "Andromeda", "Cassiopeia" }
```

### See also

- **iter_move (C++20)** — casts the result of dereferencing an object to its
  associated rvalue reference type (customization point object)
- **iter_move (C++20)** — casts the result of dereferencing the underlying
  iterator to its associated rvalue reference type (function)
- **move (C++11)** — obtains an rvalue reference (function template)
- **move_if_noexcept (C++11)** — obtains an rvalue reference if the move
  constructor does not throw (function template)
- **forward (C++11)** — forwards a function argument (function template)
- **ranges::move (C++20)** — moves a range of elements to a new location
  (niebloid)
- **ranges::move_backward (C++20)** — moves a range of elements to a new
  location in backwards order (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/iterator/common_iterator/iter_move*
