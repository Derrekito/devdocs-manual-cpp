# std::basic_ios<CharT,Traits>::rdstate

```cpp
iostate rdstate() const;
```

Returns the current stream error state.

### Parameters

(none)

### Return value

current stream error state. It is a bitmask type and can be a combination of the
following constants:

- **`goodbit`** — no error
- **`badbit`** — irrecoverable stream error
- **`failbit`** — input/output operation failed (formatting or extraction error)
- **`eofbit`** — associated input sequence has reached end-of-file

### Example

```cpp
#include <iostream>
#include <sstream>

int main()
{
    std::ostringstream stream;

    if (stream.rdstate() == std::ios_base::goodbit)
        std::cout << "stream state is goodbit\n";

    stream.setstate(std::ios_base::eofbit);

    // check state is exactly eofbit (no failbit and no badbit)
    if (stream.rdstate() == std::ios_base::eofbit)
        std::cout << "stream state is eofbit\n";
}
```

Output:

```text
stream state is goodbit
stream state is eofbit
```

### See also

- **setstate** — sets state flags (public member function)
- **clear** — modifies state flags (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ios/rdstate*
