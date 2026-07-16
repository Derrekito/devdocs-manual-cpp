# std::basic_ios<CharT,Traits>::clear

```cpp
void clear( std::ios_base::iostate state = std::ios_base::goodbit );
```

Sets the stream error state flags by assigning them the value of `state`. By
default, assigns `std::ios_base::goodbit` which has the effect of clearing all
error state flags.

If `rdbuf()` is a null pointer (i.e. there is no associated stream buffer), then
`state | std::ios_base::badbit` is assigned.

### Parameters

- **state** — new error state flags setting. It can be a combination of the
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

### Exceptions

If the new error state includes a bit that is also included in the
`exceptions()` mask, throws an exception of type `failure`.

### Example

`clear()` without arguments can be used to unset the `failbit` after unexpected
input; for `std::cin.putback(c)` see `ungetc`.

```cpp
#include <iostream>
#include <string>

int main()
{
    for (char c : {'\n', '4', '1', '.', '3', '\n', 'Z', 'Y', 'X'})
        std::cin.putback(c); // emulate user's input (not portable: see ungetc Notes)

    double n;
    while (std::cout << "Please, enter a number: " && !(std::cin >> n))
    {
        std::cin.clear();
        std::string line;
        std::getline(std::cin, line);
        std::cout << line << "\nI am sorry, but '" << line << "' is not a number\n";
    }
    std::cout << n << "\nThank you for entering the number " << n << '\n';
}
```

Output:

```text
Please, enter a number: XYZ
I am sorry, but 'XYZ' is not a number
Please, enter a number: 3.14
Thank you for entering the number 3.14
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 412 | C++98 | an excption would be thrown if the current error state
      includes a bit that is also included in the `exceptions()` mask | checks
      the new error state instead

### See also

- **setstate** — sets state flags (public member function)
- **rdstate** — returns state flags (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ios/clear*
