# std::ios_base::xalloc

```cpp
static int xalloc();
```

Returns a unique (program-wide) index value that can be used to access one
`long` and one `void*` elements in the private storage of `std::ios_base` by
calling `iword()` and `pword()`. The call to `xalloc` does not allocate memory.

This function is thread-safe; concurrent access by multiple threads does not
result in a data race.(since C++14)

Effectively increments a private static data member of `std::ios_base`, as if by
executing `return index++;`, if `index` is the name of that static member (which
may be `std::atomic` to support concurrent access by multiple threads, or
otherwise synchronized)(since C++14)

### Parameters

(none)

### Return value

Unique integer for use as pword/iword index.

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

### See also

- **pword** — resizes the private storage if necessary and access to the void*
  element at the given index (public member function)
- **iword** — resizes the private storage if necessary and access to the long
  element at the given index (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/ios_base/xalloc*
