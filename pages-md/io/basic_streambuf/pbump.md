# std::basic_streambuf<CharT,Traits>::pbump

```cpp
protected:
void pbump( int count );
```

Repositions the *put pointer* (`pptr()`) by `count` characters, where `count`
may be positive or negative. No checks are done for moving the pointer outside
the put area `[``pbase()``,``epptr()``)`.

If the pointer is advanced and then `overflow()` is called to flush the put area
to the associated character sequence, the effect is that extra `count`
characters with undefined values are output.

### Parameters

- **count** — number to add to the put pointer

### Return value

(none)

### Notes

Because this function takes an `int`, it cannot manipulate buffers larger than
`std::numeric_limits<int>::max()` characters (LWG issue 255).

### Example

```cpp
#include <fstream>
#include <iostream>
#include <string>

struct showput_streambuf : std::filebuf
{
    using std::filebuf::pbump; // expose protected
    std::string showput() const
    {
        return std::string(pbase(), pptr());
    }
};

int main()
{
    showput_streambuf mybuf;
    mybuf.open("test.txt", std::ios_base::out);
    std::ostream str(&mybuf);
    str << "This is a test" << std::flush << "1234";
    std::cout << "The put area contains: " << mybuf.showput() << '\n';
    mybuf.pbump(10);
    std::cout << "after pbump(10), it contains " << mybuf.showput() << '\n';
}
```

Output:

```text
The put area contains: 1234
after pbump(10), it contains 1234 is a test
```

### See also

- **gbump** — advances the next pointer in the input sequence (protected member
  function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/pbump*
