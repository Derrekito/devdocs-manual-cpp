# operator==,!=(std::function)

```cpp
template< class R, class... ArgTypes >
bool operator==( const std::function<R(ArgTypes...)>& f,
                 std::nullptr_t ) noexcept;  // (1) (since C++11)
template< class R, class... ArgTypes >
bool operator==( std::nullptr_t,
                 const std::function<R(ArgTypes...)>& f ) noexcept;  // (2) (since C++11) (until C++20)
template< class R, class... ArgTypes >
bool operator!=( const std::function<R(ArgTypes...)>& f,
                 std::nullptr_t ) noexcept;  // (3) (since C++11) (until C++20)
template< class R, class... ArgTypes >
bool operator!=( std::nullptr_t,
                 const std::function<R(ArgTypes...)>& f ) noexcept;  // (4) (since C++11) (until C++20)
```

Compares a `std::function` with a null pointer. Empty functions (that is,
functions without a callable target) compare equal, non-empty functions compare
non-equal.

The `!=` operator is synthesized from `operator==`.
*(since C++20)*

### Parameters

- **f** — `std::function` to compare

### Return value

1,2) `!f`

3,4) `(bool) f`

### Example

```cpp
#include <functional>
#include <iostream>

using SomeVoidFunc = std::function<void(int)>;

class C
{
public:
    C(SomeVoidFunc void_func = nullptr) : void_func_(void_func)
    {
        if (void_func_ == nullptr) // specialized compare with nullptr
            void_func_ = std::bind(&C::default_func, this, std::placeholders::_1);
        void_func_(7);
    }

    void default_func(int i) { std::cout << i << '\n'; };

private:
    SomeVoidFunc void_func_;
};

void user_func(int i)
{
    std::cout << (i + 1) << '\n';
}

int main()
{
    C c1;
    C c2(user_func);
}
```

Output:

```text
7
8
```

### See also

- **operator== (C++23)** — compares a `std::move_only_function` with `nullptr`
  (function)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/function/operator_cmp*
