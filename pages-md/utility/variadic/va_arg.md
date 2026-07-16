# va_arg

```cpp
T va_arg( std::va_list ap, T );
```

The `va_arg` macro expands to an expression of type `T` that corresponds to the
next parameter from the `va_list` `ap`.

Prior to calling `va_arg`, `ap` must be initialized by a call to either
`va_start` or `va_copy`, with no intervening call to `va_end`. Each invocation
of the `va_arg` macro modifies `ap` to point to the next variable argument.

If the type of the next argument in `ap` (after promotions) is not compatible
with `T`, the behavior is undefined, unless:

- one type is a signed integer type, the other type is the corresponding
  unsigned integer type, and the value is representable in both types; or
- one type is pointer to `void` and the other is a pointer to a character type
  (`char`, `signed char`, or `unsigned char`).

If `va_arg` is called when there are no more arguments in `ap`, the behavior is
undefined.

### Parameters

- **ap** — an instance of the `va_list` type
- **T** — the type of the next parameter in `ap`

### Expanded value

The next variable parameter in `ap`.

### Example

```cpp
#include <cstdarg>
#include <cstdio>
#include <iostream>

void print_variance(std::size_t count, const char* fmt, ...)
{
    double sum = 0;
    double sum_sq = 0;
    std::va_list args;
    va_start(args, fmt);
    for (std::size_t i = count; i--;)
    {
        double num = va_arg(args, double);
        sum += num;
        sum_sq += num*num;
    }
    va_end(args);
    std::printf(fmt, sum_sq / count - (sum / count) * (sum / count));
}

void nano_printf(const char* fmt, ...)
{
    std::va_list args;
    va_start(args, fmt);

    for (const char* p = fmt; *p != '\0'; ++p)
    {
        switch (*p)
        {
        case '%':
            switch (*++p) // read format symbol
            {
                case 'i':
                    std::cout << va_arg(args, int);
                    continue;
                case 'f':
                    std::cout << va_arg(args, double);
                    continue;
                case 's':
                    std::cout << va_arg(args, const char*);
                    continue;
                case 'c':
                    std::cout << static_cast<char>(va_arg(args, int));
                    continue;
                case '%':
                    std::cout << '%';
                    continue;
                /* ...more cases... */
            }
            break; // format error...
        case '\n':
            std::cout << '\n';
            continue;
        case '\t':
            std::cout << '\t';
            continue;
        /* ...more cases... */
        }
        std::cout << *p;
    }

    va_end(args);
}

int main()
{
    print_variance(4, "%f\n", 25.0, 27.3, 26.9, 25.7);
    nano_printf("Args: %i%% %c%f %s\n", 42, '#', 3.14, "C++");
}
```

Output:

```text
0.846875
Args: 42% #3.14 C++
```

### See also

- **va_start** — enables access to variadic function arguments (function macro)
- **va_copy (C++11)** — makes a copy of the variadic function arguments
  (function macro)
- **va_end** — ends traversal of the variadic function arguments (function
  macro)

**C documentation for `va_arg`**

---
*Source: https://en.cppreference.com/w/cpp/utility/variadic/va_arg*
