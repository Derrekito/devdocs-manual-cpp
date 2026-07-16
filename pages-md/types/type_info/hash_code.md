# std::type_info::hash_code

```cpp
std::size_t hash_code() const noexcept;  // (since C++11)
```

Returns an unspecified value (here denoted by *hash code*) such that for all
`std::type_info` objects referring to the same type, their *hash code* is the
same.

No other guarantees are given: `std::type_info` objects referring to different
types may have the same *hash code* (although the standard recommends that
implementations avoid this as much as possible), and *hash code* for the same
type can change between invocations of the same program.

### Parameters

(none)

### Return value

A value that is identical for all `std::type_info` objects referring to the same
type.

### Example

The following program is an example of an efficient type-value mapping without
using `std::type_index`.

```cpp
#include <iostream>
#include <typeinfo>
#include <unordered_map>
#include <string>
#include <functional>
#include <memory>

struct A {
    virtual ~A() {}
};

struct B : A {};
struct C : A {};

using TypeInfoRef = std::reference_wrapper<const std::type_info>;

struct Hasher {
    std::size_t operator()(TypeInfoRef code) const
    {
        return code.get().hash_code();
    }
};

struct EqualTo {
    bool operator()(TypeInfoRef lhs, TypeInfoRef rhs) const
    {
        return lhs.get() == rhs.get();
    }
};

int main()
{
    std::unordered_map<TypeInfoRef, std::string, Hasher, EqualTo> type_names;

    type_names[typeid(int)] = "int";
    type_names[typeid(double)] = "double";
    type_names[typeid(A)] = "A";
    type_names[typeid(B)] = "B";
    type_names[typeid(C)] = "C";

    int i;
    double d;
    A a;

    // note that we're storing pointer to type A
    std::unique_ptr<A> b(new B);
    std::unique_ptr<A> c(new C);

    std::cout << "i is " << type_names[typeid(i)] << '\n';
    std::cout << "d is " << type_names[typeid(d)] << '\n';
    std::cout << "a is " << type_names[typeid(a)] << '\n';
    std::cout << "*b is " << type_names[typeid(*b)] << '\n';
    std::cout << "*c is " << type_names[typeid(*c)] << '\n';
}
```

Output:

```text
i is int
d is double
a is A
*b is B
*c is C
```

### See also

- **operator==operator!= (removed in C++20)** — checks whether the objects refer
  to the same type (public member function)
- **name** — implementation defined name of the type (public member function)

---
*Source: https://en.cppreference.com/w/cpp/types/type_info/hash_code*
