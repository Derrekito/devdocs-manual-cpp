# std::exchange

```cpp
template< class T, class U = T >
T exchange( T& obj, U&& new_value );  // (since C++14) (until C++20)
template< class T, class U = T >
constexpr T exchange( T& obj, U&& new_value );  // (since C++20) (until C++23)
template< class T, class U = T >
constexpr T exchange( T& obj, U&& new_value ) noexcept(/* see below */);  // (since C++23)
```

Replaces the value of `obj` with `new_value` and returns the old value of `obj`.

### Parameters

- **obj** — object whose value to replace
- **new_value** — the value to assign to `obj`

**Type requirements**

**-`T` must meet the requirements of MoveConstructible. Also, it must be possible to move-assign objects of type `U` to objects of type `T`.**

### Return value

The old value of `obj`.

### Exceptions

(none)
*(until C++23)*

`noexcept` specification:
`noexcept( std::is_nothrow_move_constructible_v<T> &&
std::is_nothrow_assignable_v<T&, U> )`
*(since C++23)*

### Possible implementation

```cpp
template<class T, class U = T>
constexpr // Since C++20
T exchange(T& obj, U&& new_value)
    noexcept( // Since C++23
        std::is_nothrow_move_constructible<T>::value &&
        std::is_nothrow_assignable<T&, U>::value
    )
{
    T old_value = std::move(obj);
    obj = std::forward<U>(new_value);
    return old_value;
}
```

### Notes

The `std::exchange` can be used when implementing move assignment operators and
move constructors:

```cpp
struct S
{
    int n;

    S(S&& other) noexcept : n{std::exchange(other.n, 0)} {}

    S& operator=(S&& other) noexcept
    {
        if (this != &other)
            n = std::exchange(other.n, 0); // Move n, while leaving zero in other.n
        return *this;
    }
};
```

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_exchange_function` | 201304L | (C++14) | `std::exchange`

### Example

```cpp
#include <iostream>
#include <iterator>
#include <utility>
#include <vector>

class stream
{
public:
    using flags_type = int;

public:
    flags_type flags() const { return flags_; }

    // Replaces flags_ by newf, and returns the old value.
    flags_type flags(flags_type newf) { return std::exchange(flags_, newf); }

private:
    flags_type flags_ = 0;
};

void f() { std::cout << "f()"; }

int main()
{
    stream s;

    std::cout << s.flags() << '\n';
    std::cout << s.flags(12) << '\n';
    std::cout << s.flags() << "\n\n";

    std::vector<int> v;

    // Since the second template parameter has a default value, it is possible
    // to use a braced-init-list as second argument. The expression below
    // is equivalent to std::exchange(v, std::vector<int>{1, 2, 3, 4});

    std::exchange(v, {1, 2, 3, 4});

    std::copy(begin(v), end(v), std::ostream_iterator<int>(std::cout, ", "));

    std::cout << "\n\n";

    void (*fun)();

    // The default value of template parameter also makes possible to use a
    // normal function as second argument. The expression below is equivalent to
    // std::exchange(fun, static_cast<void(*)()>(f))
    std::exchange(fun, f);
    fun();

    std::cout << "\n\nFibonacci sequence: ";
    for (int a{0}, b{1}; a < 100; a = std::exchange(b, a + b))
        std::cout << a << ", ";
    std::cout << "...\n";
}
```

Output:

```text
0
0
12

1, 2, 3, 4,

f()

Fibonacci sequence: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, ...
```

### See also

- **swap** — swaps the values of two objects (function template)
- **atomic_exchangeatomic_exchange_explicit (C++11)(C++11)** — atomically
  replaces the value of the atomic object with non-atomic argument and returns
  the old value of the atomic (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/exchange*
