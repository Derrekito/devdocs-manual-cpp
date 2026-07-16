# Undefined behavior

Renders the entire program meaningless if certain rules of the language are
violated.

### Explanation

The C++ standard precisely defines the observable behavior of every C++ program
that does not fall into one of the following classes:

- *ill-formed* - the program has syntax errors or diagnosable semantic errors. A
  conforming C++ compiler is required to issue a diagnostic, even if it defines
  a language extension that assigns meaning to such code (such as with
  variable-length arrays). The text of the standard uses *shall*, *shall not*,
  and *ill-formed* to indicate these requirements.
- *ill-formed, no diagnostic required* - the program has semantic errors which
  may not be diagnosable in general case (e.g. violations of the ODR or other
  errors that are only detectable at link time). The behavior is undefined if
  such program is executed.
- *implementation-defined behavior* - the behavior of the program varies between
  implementations, and the conforming implementation must document the effects
  of each behavior. For example, the type of `std::size_t` or the number of bits
  in a byte, or the text of `std::bad_alloc::what`. A subset of
  implementation-defined behavior is *locale-specific behavior*, which depends
  on the implementation-supplied locale.
- *unspecified behavior* - the behavior of the program varies between
  implementations, and the conforming implementation is not required to document
  the effects of each behavior. For example, order of evaluation, whether
  identical string literals are distinct, the amount of array allocation
  overhead, etc. Each unspecified behavior results in one of a set of valid
  results.
- *undefined behavior* - there are no restrictions on the behavior of the
  program. Examples of undefined behavior are data races, memory accesses
  outside of array bounds, signed integer overflow, null pointer dereference,
  more than one modifications of the same scalar in an expression without any
  intermediate sequence point(until C++11)that is unsequenced(since C++11),
  access to an object through a pointer of a different type, etc. Compilers are
  not required to diagnose undefined behavior (although many simple situations
  are diagnosed), and the compiled program is not required to do anything
  meaningful.

### UB and optimization

Because correct C++ programs are free of undefined behavior, compilers may
produce unexpected results when a program that actually has UB is compiled with
optimization enabled:

For example,

#### Signed overflow

```cpp
int foo(int x)
{
    return x + 1 > x; // either true or UB due to signed overflow
}
```

may be compiled as (demo)

```cpp
foo(int):
        mov     eax, 1
        ret
```

#### Access out of bounds

```cpp
int table[4] = {};
bool exists_in_table(int v)
{
    // return true in one of the first 4 iterations or UB due to out-of-bounds access
    for (int i = 0; i <= 4; i++)
        if (table[i] == v)
            return true;
    return false;
}
```

May be compiled as (demo)

```cpp
exists_in_table(int):
        mov     eax, 1
        ret
```

#### Uninitialized scalar

```cpp
std::size_t f(int x)
{
    std::size_t a;
    if (x) // either x nonzero or UB
        a = 42;
    return a;
}
```

May be compiled as (demo)

```cpp
f(int):
        mov     eax, 42
        ret
```

The output shown was observed on an older version of gcc

```cpp
#include <cstdio>

int main()
{
    bool p; // uninitialized local variable
    if (p)  // UB access to uninitialized scalar
        std::puts("p is true");
    if (!p) // UB access to uninitialized scalar
        std::puts("p is false");
}
```

Possible output:

```text
p is true
p is false
```

#### Invalid scalar

```cpp
int f()
{
    bool b = true;
    unsigned char* p = reinterpret_cast<unsigned char*>(&b);
    *p = 10;
    // reading from b is now UB
    return b == 0;
}
```

May be compiled as (demo)

```cpp
f():
        mov     eax, 11
        ret
```

#### Null pointer dereference

It is arguable whether merely dereferencing a null pointer is undefined
behavior, see CWG issue 2823. The examples demonstrate reading from the result
of such deferencing.

```cpp
int foo(int* p)
{
    int x = *p;
    if (!p)
        return x; // Either UB above or this branch is never taken
    else
        return 0;
}
int bar()
{
    int* p = nullptr;
    return *p; // Unconditional UB
}
```

may be compiled as (demo)

```cpp
foo(int*):
        xor     eax, eax
        ret
bar():
        ret
```

#### Access to pointer passed to `std::realloc`

Choose clang to observe the output shown

```cpp
#include <cstdlib>
#include <iostream>

int main()
{
    int *p = (int*)std::malloc(sizeof(int));
    int *q = (int*)std::realloc(p, sizeof(int));
    *p = 1; // UB access to a pointer that was passed to realloc
    *q = 2;
    if (p == q) // UB access to a pointer that was passed to realloc
        std::cout << *p << *q << '\n';
}
```

Possible output:

```text
12
```

#### Infinite loop without side-effects

Choose clang or the latest gcc to observe the output shown.

```cpp
#include <iostream>

bool fermat()
{
    const int max_value = 1000;

    // Endless loop with no side effects is UB
    for (int a = 1, b = 1, c = 1; true; )
    {
        if (((a * a * a) == ((b * b * b) + (c * c * c))))
            return true; // disproved :)
        a++;
        if (a > max_value)
        {
            a = 1;
            b++;
        }
        if (b > max_value)
        {
            b = 1;
            c++;
        }
        if (c > max_value)
            c = 1;
    }

    return false; // not disproved
}

int main()
{
    std::cout << "Fermat's Last Theorem ";
    fermat()
        ? std::cout << "has been disproved!\n"
        : std::cout << "has not been disproved.\n";
}
```

Possible output:

```text
Fermat's Last Theorem has been disproved!
```

### See also

**C documentation for Undefined behavior**

### External links

  1. | The LLVM Project Blog: What Every C Programmer Should Know About
      Undefined Behavior #1/3
  2. | The LLVM Project Blog: What Every C Programmer Should Know About
      Undefined Behavior #2/3
  3. | The LLVM Project Blog: What Every C Programmer Should Know About
      Undefined Behavior #3/3
  4. | Undefined behavior can result in time travel (among other things, but
      time travel is the funkiest)
  5. | Understanding Integer Overflow in C/C++
  6. | Fun with NULL pointers, part 1 (local exploit in Linux 2.6.30 caused by
      UB due to null pointer dereference)
  7. | Undefined Behavior and Fermat’s Last Theorem.

---
*Source: https://en.cppreference.com/w/cpp/language/ub*
