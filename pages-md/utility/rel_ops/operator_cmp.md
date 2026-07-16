# std::rel_ops::operator!=,>,<=,>=

```cpp
template< class T >
bool operator!=( const T& lhs, const T& rhs );  // (1) (deprecated in C++20)
template< class T >
bool operator>( const T& lhs, const T& rhs );  // (2) (deprecated in C++20)
template< class T >
bool operator<=( const T& lhs, const T& rhs );  // (3) (deprecated in C++20)
template< class T >
bool operator>=( const T& lhs, const T& rhs );  // (4) (deprecated in C++20)
```

Given a user-defined `operator==` and `operator<` for objects of type `T`,
implements the usual semantics of other comparison operators.

1) Implements `operator!=` in terms of `operator==`.

2) Implements `operator>` in terms of `operator<`.

3) Implements `operator<=` in terms of `operator<`.

4) Implements `operator>=` in terms of `operator<`.

### Parameters

- **lhs** — left-hand argument
- **rhs** — right-hand argument

### Return value

1) Returns `true` if `lhs` is *not equal* to `rhs`.

2) Returns `true` if `lhs` is *greater* than `rhs`.

3) Returns `true` if `lhs` is *less or equal* to `rhs`.

4) Returns `true` if `lhs` is *greater or equal* to `rhs`.

### Possible implementation

```cpp
namespace rel_ops
{
    template<class T>
    bool operator!=(const T& lhs, const T& rhs)
    {
        return !(lhs == rhs);
    }
}
```

```cpp
namespace rel_ops
{
    template<class T>
    bool operator>(const T& lhs, const T& rhs)
    {
        return rhs < lhs;
    }
}
```

```cpp
namespace rel_ops
{
    template<class T>
    bool operator<=(const T& lhs, const T& rhs)
    {
        return !(rhs < lhs);
    }
}
```

```cpp
namespace rel_ops
{
    template<class T>
    bool operator>=(const T& lhs, const T& rhs)
    {
        return !(lhs < rhs);
    }
}
```

### Notes

Boost.operators provides a more versatile alternative to `std::rel_ops`.

As of C++20, `std::rel_ops` are deprecated in favor of `operator<=>`.

### Example

```cpp
#include <iostream>
#include <utility>

struct Foo
{
    int n;
};

bool operator==(const Foo& lhs, const Foo& rhs)
{
    return lhs.n == rhs.n;
}

bool operator<(const Foo& lhs, const Foo& rhs)
{
    return lhs.n < rhs.n;
}

int main()
{
    Foo f1 = {1};
    Foo f2 = {2};
    using namespace std::rel_ops;

    std::cout << std::boolalpha
              << "{1} != {2} : " << (f1 != f2) << '\n'
              << "{1} >  {2} : " << (f1 >  f2) << '\n'
              << "{1} <= {2} : " << (f1 <= f2) << '\n'
              << "{1} >= {2} : " << (f1 >= f2) << '\n';
}
```

Output:

```text
{1} != {2} : true
{1} >  {2} : false
{1} <= {2} : true
{1} >= {2} : false
```

---
*Source: https://en.cppreference.com/w/cpp/utility/rel_ops/operator_cmp*
