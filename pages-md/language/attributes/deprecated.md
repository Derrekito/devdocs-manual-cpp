# C++ attribute: deprecated (since C++14)

Indicates that the name or entity declared with this attribute is deprecated,
that is, the use is allowed, but discouraged for some reason.

### Syntax

```text
[[deprecated]]    (1)
[[deprecated( string-literal )]]    (2)
```

- **string-literal** — an unevaluated string literal that could be used to
  explain the rationale for deprecation and/or to suggest a replacing entity

### Explanation

Indicates that the use of the name or entity declared with this attribute is
allowed, but discouraged for some reason. Compilers typically issue warnings on
such uses. The string-literal, if specified, is usually included in the
warnings.

This attribute is allowed in declarations of the following names or entities:

- class/struct/union: `struct [[deprecated]] S;`,
- typedef-name, including those declared by alias declaration: `[[deprecated]]
  typedef S* PS;`, `using PS [[deprecated]] = S*;`,
- variable, including static data member: `[[deprecated]] int x;`,
- non-static data member: `union U { [[deprecated]] int n; };`,
- function: `[[deprecated]] void f();`,
- namespace: `namespace [[deprecated]] NS { int x; }`,
- enumeration: `enum [[deprecated]] E {};`,
- enumerator: `enum { A [[deprecated]], B [[deprecated]] = 42 };`, (since C++17)
- template specialization: `template<> struct [[deprecated]] X<int> {};`.

A name declared non-deprecated may be redeclared deprecated. A name declared
deprecated cannot be un-deprecated by redeclaring it without this attribute.

### Example

```cpp
#include <iostream>

[[deprecated]]
void TriassicPeriod()
{
    std::clog << "Triassic Period: [251.9 - 208.5] million years ago.\n";
}

[[deprecated("Use NeogenePeriod() instead.")]]
void JurassicPeriod()
{
    std::clog << "Jurassic Period: [201.3 - 152.1] million years ago.\n";
}

[[deprecated("Use calcSomethingDifferently(int).")]]
int calcSomething(int x)
{
    return x * 2;
}

int main()
{
    TriassicPeriod();
    JurassicPeriod();
}
```

Possible output:

```text
Triassic Period: [251.9 - 208.5] million years ago.
Jurassic Period: [201.3 - 152.1] million years ago.

main.cpp:20:5: warning: 'TriassicPeriod' is deprecated [-Wdeprecated-declarations]
    TriassicPeriod();
    ^
main.cpp:3:3: note: 'TriassicPeriod' has been explicitly marked deprecated here
[[deprecated]]
  ^
main.cpp:21:5: warning: 'JurassicPeriod' is deprecated: Use NeogenePeriod() instead [-Wdeprecated-declarations]
    JurassicPeriod();
    ^
main.cpp:8:3: note: 'JurassicPeriod' has been explicitly marked deprecated here
[[deprecated("Use NeogenePeriod() instead")]]
  ^
2 warnings generated.
```

### See also

**C documentation for `deprecated`**

---
*Source: https://en.cppreference.com/w/cpp/language/attributes/deprecated*
