# Standard library header <ciso646> (until C++20), <iso646.h>

This header was originally in the C standard library as `<iso646.h>`.

Compatibility header, in C defines alternative operator representations which
are keywords in C++.

This means that in a conforming implementation, including this header has no
effect.

### Notes

In old or nonconforming compilers, using the alternative operator
representations may still require including this header.

`<ciso646>` is removed in C++20. Corresponding `<iso646.h>` is still available
in C++20.

Prior to C++20, including `<ciso646>` was sometimes used as a technique for
obtaining definitions of implementation-specific library version macros without
causing other effects. As of C++20, the header `<version>` was added for this
purpose.

```cpp
#include <ciso646>
#ifdef _LIBCPP_VERSION
#error Using LLVM libc++
#elif __GLIBCXX__ // Note: only version 6.1 or newer define this in ciso646
#error Using GNU libstdc++
#elif _CPPLIB_VER // Note: used by Visual Studio
#error Using Microsoft STL
#else
#error Using an unknown standard library
#endif
```

Possible output:

```text
main.cpp:7:2: error: Using Microsoft STL
#error Using Microsoft STL
 ^
1 error generated.
```

---
*Source: https://en.cppreference.com/w/cpp/header/ciso646*
