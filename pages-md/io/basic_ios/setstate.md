# std::basic_ios<CharT,Traits>::setstate

```cpp
void setstate( iostate state );
```

Sets the stream error flags `state` in addition to currently set flags.
Essentially calls `clear(rdstate() | state)`. May throw an exception.

### Parameters

- **state** — stream error state flags to set. It can be a combination of the
  following constants: Constant Explanation `goodbit` no error `badbit`
  irrecoverable stream error `failbit` input/output operation failed (formatting
  or extraction error) `eofbit` associated input sequence has reached
  end-of-file
- **`goodbit`** — no error
- **`badbit`** — irrecoverable stream error
- **`failbit`** — input/output operation failed (formatting or extraction error)
- **`eofbit`** — associated input sequence has reached end-of-file

### Return value

(none)

### Example

```cpp
#include <iostream>
#include <sstream>

int main()
{
    std::ostringstream stream;

    if (!stream.fail())
        std::cout << "stream is not fail\n";

    stream.setstate(std::ios_base::failbit);

    if (stream.fail())
        std::cout << "now stream is fail\n";

    if (!stream.good())
        std::cout << "and stream is not good\n";
}
```

Output:

```text
stream is not fail
now stream is fail
and stream is not good
```

### See also

- **rdstate** — returns state flags (public member function)
- **clear** — modifies state flags (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ios/setstate*
