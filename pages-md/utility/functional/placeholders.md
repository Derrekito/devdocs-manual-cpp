# std::placeholders::_1, std::placeholders::_2, ..., std::placeholders::_N

```cpp
/*see below*/ _1;
/*see below*/ _2;
.
.
/*see below*/ _N;
```

The `std::placeholders` namespace contains the placeholder objects `[_1, ...,
_N]` where `N` is an implementation defined maximum number.

When used as an argument in a `std::bind` expression, the placeholder objects
are stored in the generated function object, and when that function object is
invoked with unbound arguments, each placeholder `_N` is replaced by the
corresponding Nth unbound argument.

Each placeholder is declared as if by `extern /*unspecified*/ _1;`.
*(until C++17)*

Implementations are encouraged to declare the placeholders as if by `inline
constexpr /*unspecified*/ _1;`, although declaring them by `extern
/*unspecified*/ _1;` is still allowed by the standard.
*(since C++17)*

The types of the placeholder objects are DefaultConstructible and
CopyConstructible, their default copy/move constructors do not throw exceptions,
and for any placeholder `_N`, the type `std::is_placeholder<decltype(_N)>` is
defined, where `std::is_placeholder<decltype(_N)>` is derived from
`std::integral_constant<int, N>`.

### Example

The following code shows the creation of function objects with placeholder
arguments.

```cpp
#include <functional>
#include <iostream>
#include <string>

void goodbye(const std::string& s)
{
    std::cout << "Goodbye " << s << '\n';
}

class Object
{
public:
    void hello(const std::string& s)
    {
        std::cout << "Hello " << s << '\n';
    }
};

int main()
{
    using namespace std::placeholders;

    using ExampleFunction = std::function<void(const std::string&)>;
    Object instance;
    std::string str("World");

    ExampleFunction f = std::bind(&Object::hello, &instance, _1);
    f(str); // equivalent to instance.hello(str)

    f = std::bind(&goodbye, std::placeholders::_1);
    f(str); // equivalent to goodbye(str)

    auto lambda = [](std::string pre, char o, int rep, std::string post)
    {
        std::cout << pre;
        while (rep-- > 0)
            std::cout << o;
        std::cout << post << '\n';
    };

    // binding the lambda:
    std::function<void(std::string, char, int, std::string)> g =
        std::bind(&decltype(lambda)::operator(), &lambda, _1, _2, _3, _4);
    g("G", 'o', 'o'-'g', "gol");
}
```

Output:

```text
Hello World
Goodbye World
Goooooooogol
```

### See also

- **bind (C++11)** — binds one or more arguments to a function object (function
  template)
- **is_placeholder (C++11)** — indicates that an object is a standard
  placeholder or can be used as one (class template)
- **ignore (C++11)** — placeholder to skip an element when unpacking a `tuple`
  using `tie` (constant)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/placeholders*
