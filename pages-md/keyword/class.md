# C++ keyword: class

### Usage

- declaration of a class

- declaration of a scoped enumeration type
*(since C++11)*

- In a template declaration, `class` can be used to introduce type template
  parameters and template template parameters
- If a function or a variable exists in scope with the name identical to the
  name of a class type, `class` can be prepended to the name for disambiguation,
  resulting in an elaborated type specifier

### Example

```cpp
class Foo; // forward declaration of a class

class Bar  // definition of a class
{
public:
    Bar(int i) : m_i(i) {}
private:
    int m_i;
};

template<class T> // template argument
void qux()
{
    T t;
}

enum class Pub // scoped enum, since C++11
{
    b, d, p, q
};

int main()
{
    Bar Bar(1); // variable Bar hides type Bar
    Bar Bar2(2); // compiler error
    class Bar Bar3(3); // elaborated type
}
```

---
*Source: https://en.cppreference.com/w/cpp/keyword/class*
