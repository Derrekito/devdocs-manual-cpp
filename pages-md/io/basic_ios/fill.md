# std::basic_ios<CharT,Traits>::fill

```cpp
CharT fill() const;  // (1)
CharT fill( CharT ch );  // (2)
```

Manages the fill character used to pad the output conversions to the specified
field width.

1) Returns the current fill character.

2) Sets the fill character to `ch`, returns previous value of the fill
   character.

### Parameters

- **ch** — the character to use as fill character

### Return value

The fill character before the call to the function.

### Example

```cpp
#include <iomanip>
#include <iostream>

int main ()
{
    std::cout << "With default setting : [" << std::setw(10) << 40 << "]\n";
    char prev = std::cout.fill('x');
    std::cout << "Replaced '" << prev << "' with '"
              << std::cout.fill() << "': [" << std::setw(10) << 40 << "]\n";
}
```

Output:

```text
With default setting : [        40]
Replaced ' ' with 'x': [xxxxxxxx40]
```

### See also

- **setfill** — changes the fill character (function template)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ios/fill*
