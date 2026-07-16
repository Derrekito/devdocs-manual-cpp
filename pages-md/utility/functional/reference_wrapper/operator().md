# std::reference_wrapper<T>::operator()

```cpp
template< class... ArgTypes >
typename std::result_of<T&(ArgTypes&&...)>::type
    operator() ( ArgTypes&&... args ) const;  // (since C++11) (until C++17)
template< class... ArgTypes >
std::invoke_result_t<T&, ArgTypes...>
    operator() ( ArgTypes&&... args ) const noexcept(/* see below */);  // (since C++17) (until C++20)
template< class... ArgTypes >
constexpr std::invoke_result_t<T&, ArgTypes...>
    operator() ( ArgTypes&&... args ) const noexcept(/* see below */);  // (since C++20)
```

Calls the Callable object, reference to which is stored, as if by `INVOKE(get(),
std::forward<ArgTypes>(args)...)`. This function is available only if the stored
reference points to a Callable object.

`T` must be a complete type.

### Parameters

- **args** — arguments to pass to the called function

### Return value

The return value of the called function.

### Exceptions

May throw implementation-defined exceptions.
*(since C++11)(until C++17)*

`noexcept` specification:
`noexcept(std::is_nothrow_invocable_v<T&, ArgTypes...>)`
*(since C++17)*

### Example

```cpp
#include <functional>
#include <iostream>

void f1()
{
    std::cout << "reference to function called\n";
}

void f2(int n)
{
    std::cout << "bind expression called with " << n << " as the argument\n";
}

int main()
{
    std::reference_wrapper<void()> ref1 = std::ref(f1);
    ref1();

    auto b = std::bind(f2, std::placeholders::_1);
    auto ref2 = std::ref(b);
    ref2(7);

    auto c = []{ std::cout << "lambda function called\n"; };
    auto ref3 = std::ref(c);
    ref3();
}
```

Output:

```text
reference to function called
bind expression called with 7 as the argument
lambda function called
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3764 | C++17 | operator() is not noexcept | propagate noexcept

### See also

- **getoperator T&** — accesses the stored reference (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/reference_wrapper/operator()*
