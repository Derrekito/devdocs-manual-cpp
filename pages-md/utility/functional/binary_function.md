# std::binary_function

```cpp
template<
    class Arg1,
    class Arg2,
    class Result
> struct binary_function;  // (deprecated in C++11) (removed in C++17)
```

`std::binary_function` is a base class for creating function objects with two
arguments.

`std::binary_function` does not define `operator()`; it is expected that derived
classes will define this. `std::binary_function` provides only three types -
`first_argument_type`, `second_argument_type` and `result_type` - defined by the
template parameters.

Some standard library function object adaptors, such as `std::not2`, require the
function objects they adapt to have certain types defined; `std::not2` requires
the function object being adapted to have two types named `first_argument_type`
and `second_argument_type`. Deriving function objects that take two arguments
from `std::binary_function` is an easy way to make them compatible with those
adaptors.

`std::binary_function` is deprecated in C++11 and removed in C++17.

### Member types

- **`first_argument_type`** — `Arg1`
- **`second_argument_type`** — `Arg2`
- **`result_type`** — `Result`

### Example

```cpp
#include <algorithm>
#include <functional>
#include <iostream>
#include <vector>

struct same : std::binary_function<int, int, bool>
{
    bool operator()(int a, int b) const { return a == b; }
};

int main()
{
    std::vector<char> v1{'A', 'B', 'C', 'D', 'E'};
    std::vector<char> v2{'E', 'D', 'C', 'B', 'A'};
    std::vector<bool> v3(v1.size());

    std::transform(v1.begin(), v1.end(), v2.begin(), v3.begin(), std::not2(same()));

    std::cout << std::boolalpha;
    for (std::size_t i = 0; i < v1.size(); ++i)
        std::cout << v1[i] << " != " << v2[i] << " : " << v3[i] << '\n';
}
```

Output:

```text
A != E : true
B != D : true
C != C : false
D != B : true
E != A : true
```

### See also

- **function (C++11)** — wraps callable object of any copy constructible type
  with specified function call signature (class template)
- **move_only_function (C++23)** — wraps callable object of any type with
  specified function call signature (class template)
- **ptr_fun (deprecated in C++11)(removed in C++17)** — creates an
  adaptor-compatible function object wrapper from a pointer to function
  (function template)
- **pointer_to_binary_function (deprecated in C++11)(removed in C++17)** —
  adaptor-compatible wrapper for a pointer to binary function (class template)
- **unary_function (deprecated in C++11)(removed in C++17)** —
  adaptor-compatible unary function base class (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/binary_function*
