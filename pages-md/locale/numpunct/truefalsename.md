# std::numpunct<CharT>::truename, do_truename, falsename, do_falsename

```cpp
public:
string_type truename() const;  // (1)
public:
string_type falsename() const;  // (2)
protected:
virtual string_type do_truename() const;  // (3)
protected:
virtual string_type do_falsename() const;  // (4)
```

1,2) Public member function, calls the member function `do_truename` and
   `do_falsename` of the most derived class respectively.

3) Returns the string to be used as the representation of the boolean value
   `true`.

4) Returns the string to be used as the representation of the boolean value
   `false`.

### Return value

1,3) The object of type `string_type` to use as the representation of `true`.
   The standard specializations of `std::numpunct` return `"true"` and
   `L"true"`.

2,4) The object of type `string_type` to use as the representation of `false`.
   The standard specializations of `std::numpunct` return `"false"` and
   `L"false"`.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <locale>

struct custom_tf : std::numpunct<char>
{
    std::string do_truename()  const { return {'t'}; }
    std::string do_falsename() const { return {'f'}; }
};

int main()
{
    std::cout << std::boolalpha;

    // default boolalpha output
    std::cout << "Default locale,\n"
                 "  boolalpha  true: " << true << "\n"
                 "  boolalpha false: " << false << "\n\n";

    // with custom_tf applied to locale
    std::cout.imbue(std::locale(std::cout.getloc(), new custom_tf));
    std::cout << "Locale with modified numpunct,\n"
                 "  boolalpha  true: " << true << "\n"
                 "  boolalpha false: " << false << '\n';
}
```

Output:

```text
Default locale,
  boolalpha  true: true
  boolalpha false: false

Locale with modified numpunct,
  boolalpha  true: t
  boolalpha false: f
```

---
*Source: https://en.cppreference.com/w/cpp/locale/numpunct/truefalsename*
