# std::basic_streambuf<CharT,Traits>::underflow

```cpp
protected:
virtual int_type underflow();
```

Ensures that at least one character is available in the input area by updating
the pointers to the input area (if needed) and reading more data in from the
input sequence (if applicable). Returns the value of that character (converted
to `int_type` with `Traits::to_int_type(c)`) on success or `Traits::eof()` on
failure.

The function may update `gptr`, `egptr` and `eback` pointers to define the
location of newly loaded data (if any). On failure, the function ensures that
either `gptr() == nullptr` or `gptr() == egptr`.

The base class version of the function does nothing. The derived classes may
override this function to allow updates to the get area in the case of
exhaustion.

### Parameters

(none)

### Return value

The value of the character pointed to by the *get pointer* after the call on
success, or `Traits::eof()` otherwise.

The base class version of the function returns `traits::eof()`.

### Note

The public functions of `std::streambuf` call this function only if `gptr() ==
nullptr` or `gptr() >= egptr()`.

### Example

```cpp
#include <iostream>
#include <sstream>

class null_filter_buf : public std::streambuf
{
    std::streambuf* src;
    char ch; // single-byte buffer
protected:
    int underflow()
    {
        traits_type::int_type i;
        while ((i = src->sbumpc()) == '\0')
            ; // skip zeroes
        if (!traits_type::eq_int_type(i, traits_type::eof()))
        {
            ch = traits_type::to_char_type(i);
            setg(&ch, &ch, &ch+1); // make one read position available
        }
        return i;
    }
public:
    null_filter_buf(std::streambuf* buf) : src(buf)
    {
        setg(&ch, &ch + 1, &ch + 1); // buffer is initially full
    }
};

void filtered_read(std::istream& in)
{
    std::streambuf* orig = in.rdbuf();
    null_filter_buf buf(orig);
    in.rdbuf(&buf);
    for (char c; in.get(c);)
        std::cout << c;
    in.rdbuf(orig);
}

int main()
{
    char a[] = "This i\0s \0an e\0\0\0xample";
    std::istringstream in(std::string(std::begin(a), std::end(a)));
    filtered_read(in);
}
```

Output:

```text
This is an example
```

### See also

- **uflow [virtual]** — reads characters from the associated input sequence to
  the get area and advances the next pointer (virtual protected member function)
- **overflow [virtual]** — writes characters to the associated output sequence
  from the put area (virtual protected member function)
- **underflow [virtual]** — reads from the associated file (virtual protected
  member function of `std::basic_filebuf<CharT,Traits>`)
- **underflow [virtual]** — returns the next character available in the input
  sequence (virtual protected member function of
  `std::basic_stringbuf<CharT,Traits,Allocator>`)
- **underflow [virtual]** — reads a character from the input sequence without
  advancing the next pointer (virtual protected member function of
  `std::strstreambuf`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/underflow*
