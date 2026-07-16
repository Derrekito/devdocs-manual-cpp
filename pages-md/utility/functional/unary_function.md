# std::unary_function

```cpp
template< typename ArgumentType, typename ResultType >
struct unary_function;  // (deprecated in C++11) (removed in C++17)
```

`std::unary_function` is a base class for creating function objects with one
argument.

`std::unary_function` does not define `operator()`; it is expected that derived
classes will define this. `std::unary_function` provides only two types -
`argument_type` and `result_type` - defined by the template parameters.

Some standard library function object adaptors, such as `std::not1`, require the
function objects they adapt to have certain types defined; `std::not1` requires
the function object being adapted to have a type named `argument_type`. Deriving
function objects that take one argument from `std::unary_function` is an easy
way to make them compatible with those adaptors.

`std::unary_function` is deprecated in C++11.

### Member types

- **`argument_type`** — `ArgumentType`
- **`result_type`** — `ResultType`

### Example

```cpp
#include <algorithm>
#include <functional>
#include <iostream>
#include <vector>

struct less_than_7 : std::unary_function<int, bool>
{
    bool operator()(int i) const { return i < 7; }
};

int main()
{
    std::vector<int> v(10, 7);
    v[0] = v[1] = v[2] = 6;

    std::cout << std::count_if(v.begin(), v.end(), std::not1(less_than_7()));

    // C++11 solution:
    // Cast to std::function<bool (int)> somehow - even with a lambda
    // std::cout << std::count_if(v.begin(), v.end(),
    //     std::not1(std::function<bool (int)>([](int i) { return i < 7; })));
}
```

Output:

```text
7
```

### See also

- **function (C++11)** — wraps callable object of any copy constructible type
  with specified function call signature (class template)
- **move_only_function (C++23)** — wraps callable object of any type with
  specified function call signature (class template)
- **ptr_fun (deprecated in C++11)(removed in C++17)** — creates an
  adaptor-compatible function object wrapper from a pointer to function
  (function template)
- **pointer_to_unary_function (deprecated in C++11)(removed in C++17)** —
  adaptor-compatible wrapper for a pointer to unary function (class template)
- **binary_function (deprecated in C++11)(removed in C++17)** —
  adaptor-compatible binary function base class (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/unary_function*
