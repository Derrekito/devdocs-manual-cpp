# std::basic_streambuf<CharT,Traits>::setp

```cpp
protected:
void setp( char_type* pbeg, char_type* pend );
```

Sets the values of the pointers defining the put area. Specifically, after the
call `pbase() == pbeg`, `pptr() == pbeg`, `epptr() == pend`.

### Parameters

- **pbeg** — pointer to the new beginning of the put area
- **pend** — pointer to the new end of the put area

### Return value

(none)

### Example

```cpp
#include <array>
#include <cstddef>
#include <iostream>

// Buffer for std::ostream implemented by std::array
template<std::size_t SIZE, class CharT = char>
class ArrayedStreamBuffer : public std::basic_streambuf<CharT>
{
public:
    using Base = std::basic_streambuf<CharT>;
    using char_type = typename Base::char_type;

    ArrayedStreamBuffer() : buffer_{} // value-initialize buffer_ to all zeroes
    {
        Base::setp(buffer_.begin(), buffer_.end()); // set std::basic_streambuf
            // put area pointers to work with 'buffer_'
    }

    void print_buffer()
    {
        for (const auto& i : buffer_)
        {
            if (i == 0)
                std::cout << "\\0";
            else
                std::cout << i;
            std::cout << ' ';
        }
        std::cout << '\n';
    }

private:
    std::array<char_type, SIZE> buffer_;
};

int main()
{
    ArrayedStreamBuffer<10> streambuf;
    std::ostream stream(&streambuf);

    stream << "hello";
    stream << ",";

    streambuf.print_buffer();
}
```

Output:

```text
h e l l o , \0 \0 \0 \0
```

### See also

- **setg** — repositions the beginning, next, and end pointers of the input
  sequence (protected member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/setp*
