# std::rethrow_if_nested

```cpp
template< class E >
void rethrow_if_nested( const E& e );  // (since C++11)
```

If `E` is not a polymorphic class type, or if `std::nested_exception` is an
inaccessible or ambiguous base class of `E`, there is no effect.

Otherwise, performs

```cpp
if (auto p = dynamic_cast<const std::nested_exception*>(std::addressof(e)))
    p->rethrow_nested();
```

### Parameters

- **e** — the exception object to rethrow

### Return value

(none)

### Notes

Unlike many related functions, this function is *not* intended to be called with
a `std::exception_ptr` but rather an actual exception reference.

### Possible implementation

```cpp
namespace details
{
    template<class E>
    struct can_dynamic_cast
        : std::integral_constant<bool,
              std::is_polymorphic<E>::value &&
              (!std::is_base_of<std::nested_exception, E>::value ||
                std::is_convertible<E*, std::nested_exception*>::value)
          > {};

    template<class T>
    void rethrow_if_nested_impl(const T& e, std::true_type)
    {
        if (auto nep = dynamic_cast<const std::nested_exception*>(std::addressof(e)))
            nep->rethrow_nested();
    }

    template<class T>
    void rethrow_if_nested_impl(const T&, std::false_type) {}
}

template<class T>
void rethrow_if_nested(const T& t)
{
    details::rethrow_if_nested_impl(t, details::can_dynamic_cast<T>());
}
```

### Example

Demonstrates construction and recursion through a nested exception object.

```cpp
#include <exception>
#include <fstream>
#include <iostream>
#include <stdexcept>
#include <string>

// prints the explanatory string of an exception. If the exception is nested,
// recurses to print the explanatory of the exception it holds
void print_exception(const std::exception& e, int level =  0)
{
    std::cerr << std::string(level, ' ') << "exception: " << e.what() << '\n';
    try
    {
        std::rethrow_if_nested(e);
    }
    catch (const std::exception& nestedException)
    {
        print_exception(nestedException, level + 1);
    }
    catch (...) {}
}

// sample function that catches an exception and wraps it in a nested exception
void open_file(const std::string& s)
{
    try
    {
        std::ifstream file(s);
        file.exceptions(std::ios_base::failbit);
    }
    catch (...)
    {
        std::throw_with_nested(std::runtime_error("Couldn't open " + s));
    }
}

// sample function that catches an exception and wraps it in a nested exception
void run()
{
    try
    {
        open_file("nonexistent.file");
    }
    catch (...)
    {
        std::throw_with_nested(std::runtime_error("run() failed"));
    }
}

// runs the sample function above and prints the caught exception
int main()
{
    try
    {
        run();
    }
    catch (const std::exception& e)
    {
        print_exception(e);
    }
}
```

Possible output:

```text
exception: run() failed
 exception: Couldn't open nonexistent.file
  exception: basic_ios::clear
```

### See also

- **nested_exception (C++11)** — a mixin type to capture and store current
  exceptions (class)
- **throw_with_nested (C++11)** — throws its argument with
  `std::nested_exception` mixed in (function template)

---
*Source: https://en.cppreference.com/w/cpp/error/rethrow_if_nested*
