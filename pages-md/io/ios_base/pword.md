# std::ios_base::pword

```cpp
void*& pword( int index );
```

First, allocates or resizes the private storage (dynamic array of `void*` or
another indexable data structure) sufficiently to make `index` a valid index,
then returns a reference to the `void*` element of the private storage with the
index `index`.

The reference may be invalidated by any operation on this `ios_base` object,
including another call to `pword()`, but the stored values are retained, so that
reading from `pword(index)` with the same index later will produce the same
value until the next call to `std::basic_ios::copyfmt()`. The value can be used
for any purpose. The index of the element must be obtained by `xalloc()`,
otherwise the behavior is undefined. New elements are initialized to a null
pointer.

If the function fails (possibly caused by an allocation failure) and `*this` is
a base class subobject of a `basic_ios<>` object or subobject, calls
`std::basic_ios<>::setstate(badbit)` which may throw `std::ios_base::failure`.

### Parameters

- **index** — index value of the element

### Return value

A reference to the element.

### Exceptions

May throw `std::ios_base::failure` when setting the badbit.

### Notes

If the pointers stored in `pword` require management, `register_callback()` may
be used to install handlers that execute deep copy or deallocation as needed.

### Example

Uses base class pword storage for runtime type identification of derived stream
objects.

```cpp
#include <iostream>

template<class CharT, class Traits = std::char_traits<CharT>>
class mystream : public std::basic_ostream<CharT, Traits>
{
public:
    static const int xindex;

    mystream(std::basic_ostream<CharT, Traits>& ostr) :
        std::basic_ostream<CharT, Traits>(ostr.rdbuf())
    {
        this->pword(xindex) = this;
    }

    void myfn()
    {
        *this << "[special handling for mystream]";
    }
};

// Each specialization of mystream obtains a unique index from xalloc()
template<class CharT, class Traits>
const int mystream<CharT, Traits>::xindex = std::ios_base::xalloc();

// This I/O manipulator will be able to recognize ostreams that are mystreams
// by looking up the pointer stored in pword
template<class CharT, class Traits>
std::basic_ostream<CharT, Traits>& mymanip(std::basic_ostream<CharT, Traits>& os)
{
    if (os.pword(mystream<CharT, Traits>::xindex) == &os)
        static_cast<mystream<CharT, Traits>&>(os).myfn();
    return os;
}

int main()
{
    std::cout << "cout, narrow-character test " << mymanip << '\n';

    mystream<char> myout(std::cout);
    myout << "myout, narrow-character test " << mymanip << '\n';

    std::wcout << "wcout, wide-character test " << mymanip << '\n';

    mystream<wchar_t> mywout(std::wcout);
    mywout << "mywout, wide-character test " << mymanip << '\n';
}
```

Output:

```text
cout, narrow-character test
myout, narrow-character test [special handling for mystream]
wcout, wide-character test
mywout, wide-character test [special handling for mystream]
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 36 | C++98 | the stored value might not be retained if the reference is
      invalidated | the stored value is retained until the next call of
      `copyfmt()`
  LWG 41 | C++98 | the function set badbit by itself on failure, but `ios_base`
      does not provide such interface | badbit is set by `basic_ios` (if `*this`
      is its base class subobject)

### See also

- **iword** — resizes the private storage if necessary and access to the long
  element at the given index (public member function)
- **xalloc [static]** — returns a program-wide unique integer that is safe to
  use as index to **`pword()`** and `iword()` (public static member function)

---
*Source: https://en.cppreference.com/w/cpp/io/ios_base/pword*
