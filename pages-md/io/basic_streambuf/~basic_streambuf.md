# std::basic_streambuf<CharT,Traits>::~basic_streambuf

```cpp
virtual ~basic_streambuf();
```

This destructor has no effect: the members of this `basic_streambuf` (the
pointers and the locale) are destructed in accordance with the usual object
destruction sequence after this destructor returns. However, since it is
declared public virtual, it allows the objects that are derived from
`std::basic_streambuf` to be deleted through a pointer to base class.

### Parameters

(none)

### Example

```cpp
#include <fstream>
#include <iostream>

int main()
{
    std::filebuf* fbp = new std::filebuf;
    fbp->open("test.txt", std::ios_base::out);
    fbp->sputn("Hello\n", 6);

    std::streambuf* sbp = fbp;
    delete sbp; // the file is closed, output flushed and written

    std::ifstream f("test.txt");
    std::cout << f.rdbuf(); // proof
}
```

Output:

```text
Hello
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 54 | C++98 | the effect of the destructor was not specified | specified as
      no effect

### See also

- **(constructor)** — constructs a `basic_streambuf` object (protected member
  function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/%7Ebasic_streambuf*
