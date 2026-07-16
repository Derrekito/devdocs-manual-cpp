# std::source_location::function_name

```cpp
constexpr const char* function_name() const noexcept;  // (since C++20)
```

Returns the name of the function associated with the position represented by
this object, if any.

### Parameters

(none)

### Return value

If this object represents a position in a body of a function, returns an
implementation-defined null-terminated byte string corresponding to the name of
the function.

Otherwise, an empty string is returned.

### Example

`std::source_location::function_name` may help to obtain the names of functions
(including the special functions) alongside with their signatures.

```cpp
#include <cstdio>
#include <utility>
#include <source_location>

inline void print_function_name(
    const std::source_location& location = std::source_location::current())
{
    std::puts(location.function_name()); // prints the name of the caller
}

void foo(double &&) { print_function_name(); }

namespace bar { void baz() { print_function_name(); } }

template <typename T> auto pub(T) { print_function_name(); return 42; }

struct S {
    S() { print_function_name(); }
    S(int) { print_function_name(); }
    ~S() { print_function_name(); }
    S& operator=(S const&) { print_function_name(); return *this; }
    S& operator=(S&&) { print_function_name(); return *this; }
};

int main(int, char const* const[])
{
    print_function_name();
    foo(3.14);
    bar::baz();
    pub(0xFULL);
    S p;
    S q{42};
    p = q;
    p = std::move(q);
    [] { print_function_name(); }();
}
```

Possible output:

```text
int main(int, const char* const*)
void foo(double&&)
void bar::baz()
auto pub(T) [with T = long long unsigned int]
S::S()
S::S(int)
S& S::operator=(const S&)
S& S::operator=(S&&)
main(int, const char* const*)::<lambda()>
S::~S()
S::~S()
```

### See also

- **line** — return the line number represented by this object (public member
  function)
- **column** — return the column number represented by this object (public
  member function)
- **file_name** — return the file name represented by this object (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/utility/source_location/function_name*
