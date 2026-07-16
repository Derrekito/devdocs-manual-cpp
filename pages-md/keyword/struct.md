# C++ keyword: struct

### Usage

- declaration of a compound type

- declaration of a scoped enumeration type
*(since C++11)*

- If a function or a variable exists in scope with the name identical to the
  name of a non-union class type, `struct` can be prepended to the name for
  disambiguation, resulting in an elaborated type specifier.

### Example

```cpp
struct Foo; // forward declaration of a struct

struct Bar  // definition of a struct
{
    Bar(int i) : i(i + i) {}

    int i;
};

enum struct Pub // scoped enum, since C++11
{
    b, d, p, q,
};

int main()
{
    Bar Bar(1);
    struct Bar Bar2(2); // elaborated type
}
```

---
*Source: https://en.cppreference.com/w/cpp/keyword/struct*
