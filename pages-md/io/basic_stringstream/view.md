# std::basic_stringstream<CharT,Traits,Allocator>::view

```cpp
std::basic_string_view<CharT, Traits> view() const noexcept;  // (since C++20)
```

Obtains a `std::basic_string_view` over the underlying string object. Equivalent
to `return rdbuf()->view();`.

### Parameters

(none)

### Return value

A `std::basic_string_view` over the underlying string object.

### Example

```cpp
#include <iostream>
#include <sstream>

int main()
{
    // input/output stream
    std::stringstream buf1;
    buf1 << 69;
    int n = 0;
    buf1 >> n;
    std::cout << "1) buf1 = [" << buf1.view() << "], n = " << n << '\n';

    // output stream in append mode
    std::ostringstream buf2("test", std::ios_base::ate);
    buf2 << '1';
    std::cout << "2) buf2 = [" << buf2.view() << "]\n";

    // input stream
    std::istringstream inbuf("-42");
    inbuf >> n;
    std::cout << "3) inbuf = [" << inbuf.view() << "], n = " << n << '\n';
}
```

Output:

```text
1) buf1 = [69], n = 69
2) buf2 = [test1]
3) inbuf = [-42], n = -42
```

### See also

- **view (C++20)** — obtains a view over the underlying character sequence
  (public member function of `std::basic_stringbuf<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_stringstream/view*
