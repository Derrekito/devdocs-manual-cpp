# std::basic_ios<CharT,Traits>::fail

```cpp
bool fail() const;
```

Returns `true` if an error has occurred on the associated stream. Specifically,
returns `true` if `badbit` or `failbit` is set in `rdstate()`.

See `ios_base::iostate` for the list of conditions that set `failbit` or
`badbit`.

### Parameters

(none)

### Return value

`true` if an error has occurred, `false` otherwise.

### Example

```cpp
#include <cstdlib>
#include <fstream>
#include <iostream>

int main()
{
    std::ifstream file("test.txt");
    if (!file) // operator! is used here
    {
        std::cout << "File opening failed\n";
        return EXIT_FAILURE;
    }

    // typical C++ I/O loop uses the return value of the I/O function
    // as the loop controlling condition, operator bool() is used here
    for (int n; file >> n;)
       std::cout << n << ' ';
    std::cout << '\n';

    if (file.bad())
        std::cout << "I/O error while reading\n";
    else if (file.eof())
        std::cout << "End of file reached successfully\n";
    else if (file.fail())
        std::cout << "Non-integer data encountered\n";
}
```

### See also

The following table shows the value of `basic_ios` accessors (`good()`,
**`fail()`**, etc.) for all possible combinations of `ios_base::iostate` flags:

  `ios_base::iostate` flags | `basic_ios` accessors
  `eofbit` | `failbit` | `badbit` | `good()` | **`fail()`** | `bad()` | `eof()`
      | `operator bool` | `operator!`
  false | false | false | true | false | false | false | true | false
  false | false | true | false | true | true | false | false | true
  false | true | false | false | true | false | false | false | true
  false | true | true | false | true | true | false | false | true
  true | false | false | false | false | false | true | true | false
  true | false | true | false | true | true | true | false | true
  true | true | false | false | true | false | true | false | true
  true | true | true | false | true | true | true | false | true

- **ferror** — checks for a file error (function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ios/fail*
