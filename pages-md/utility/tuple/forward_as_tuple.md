# std::forward_as_tuple

```cpp
template< class... Types >
tuple<Types&&...> forward_as_tuple( Types&&... args ) noexcept;  // (since C++11) (constexpr since C++14)
```

Constructs a tuple of references to the arguments in `args` suitable for
forwarding as an argument to a function. The tuple has rvalue reference data
members when rvalues are used as arguments, and otherwise has lvalue reference
data members.

### Parameters

- **args** — zero or more arguments to construct the tuple from

### Return value

A `std::tuple` object created as if by
`std::tuple<Types&&...>(std::forward<Types>(args)...)`

### Notes

If the arguments are temporaries, `forward_as_tuple` does not extend their
lifetime; they have to be used before the end of the full expression.

### Example

```cpp
#include <iostream>
#include <map>
#include <tuple>
#include <string>

int main()
{
    std::map<int, std::string> m;

    m.emplace(std::piecewise_construct,
              std::forward_as_tuple(10),
              std::forward_as_tuple(20, 'a'));
    std::cout << "m[10] = " << m[10] << '\n';

    // The following is an error: it produces a
    // std::tuple<int&&, char&&> holding two dangling references.
    //
    // auto t = std::forward_as_tuple(20, 'a');
    // m.emplace(std::piecewise_construct, std::forward_as_tuple(10), t);
}
```

Output:

```text
m[10] = aaaaaaaaaaaaaaaaaaaa
```

### See also

- **make_tuple (C++11)** — creates a `tuple` object of the type defined by the
  argument types (function template)
- **tie (C++11)** — creates a `tuple` of lvalue references or unpacks a tuple
  into individual objects (function template)
- **tuple_cat (C++11)** — creates a `tuple` by concatenating any number of
  tuples (function template)
- **apply (C++17)** — calls a function with a tuple of arguments (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/utility/tuple/forward_as_tuple*
