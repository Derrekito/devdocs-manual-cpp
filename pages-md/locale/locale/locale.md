# std::locale::locale

```cpp
locale() throw();  // (until C++11)
locale() noexcept;  // (since C++11)
locale( const locale& other ) throw();  // (until C++11)
locale( const locale& other ) noexcept;  // (since C++11)
explicit locale( const char* std_name );  // (3)
explicit locale( const std::string& std_name );  // (4) (since C++11)
locale( const locale& other, const char* std_name, category cats );  // (5)
locale( const locale& other, const std::string& std_name, category cats );  // (6) (since C++11)
template< class Facet >
locale( const locale& other, Facet* f );  // (7)
locale( const locale& other, const locale& one, category cats );  // (8)
```

Constructs a new locale object.

1) Default constructor. Constructs a copy of the global C++ locale, which is the
   locale most recently used as the argument to `std::locale::global` or a copy
   of `std::locale::classic()` if no call to `std::locale::global` has been
   made.

2) Copy constructor. Constructs a copy of `other`.

3,4) Constructs a copy of the system locale with specified `std_name` (such as
   `"C"`, or `"POSIX"`, or `"en_US.UTF-8"`, or `"English_US.1251"`), if such
   locale is supported by the operating system. The locale constructed in this
   manner has a name.

4) Equivalent to `locale(std_name.c_str())`.

5) Constructs a copy of `other` except for all the facets identified by the
   `cats` argument, which are copied from the system locale identified by its
   `std_name`. The locale constructed in this manner has a name if and only if
   `other` has a name.

6) Equivalent to `locale(other, std_name.c_str(), cats)`.

7) Constructs a copy of `other` except for the facet of type `Facet` (typically
   deduced from the type of the argument) which is installed from `f`. If `f` is
   a null pointer, the constructed locale is a full copy of `other`. The program
   is ill-formed if `Facet` is not a facet or it is a volatile-qualified facet.

If `f` is null, the constructed locale has the same name as `other`. Otherwise,
   the constructed locale has no name.

8) Constructs a copy of `other` except for all the facets identified by the
   `cats` argument, which are copied from `one`.

If `cats` is equal to `locale::none`, the constructed locale has a name if and
   only if `other` has a name. Otherwise, the constructed locale has a name if
   and only if `other` and `one` both have names.

### Parameters

- **other** — another locale to copy
- **std_name** — name of the system locale to use
- **f** — pointer to a facet to merge with `other`
- **cats** — the facet categories used to identify the facets to merge with
  `other`
- **one** — another locale to take facets from

### Exceptions

3,5) `std::runtime_error` if the operating system has no locale named `std_name`
   or if `std_name` is a null pointer.

4,6) `std::runtime_error` if the operating system has no locale named
   `std_name`.

### Notes

Overload (7) is typically called with its second argument, `f`, obtained
directly from a new-expression: the locale is responsible for calling the
matching delete from its own destructor.

### Example

```cpp
#include <codecvt>
#include <iostream>
#include <locale>

std::ostream& operator<< (std::ostream& os, std::locale const& loc)
{
    if (loc.name().length() <= 80)
        return os << loc.name() << '\n';

    for (const auto c : loc.name())
        c != ';' ? os << c : os << "\n    ";
    return os << '\n';
}

int main()
{
    // l1 is a copy of the classic "C" locale
    std::locale l1;
    // l2 is a unicode locale
    std::locale l2("en_US.UTF-8");
    // l3 is "C" except for ctype, which is unicode
    std::locale l3(l1, l2, std::locale::ctype);
    // l4 is "C" except for codecvt
    std::locale l4(l1, new std::codecvt_utf8<wchar_t>);

    std::cout
        << "Locale names:\n"
        << "l1: " << l1 << "l2: " << l2
        << "l3: " << l3 << "l4: " << l4;
}
```

Possible output:

```text
Locale names:
l1: C
l2: en_US.UTF-8
l3: LC_CTYPE=en_US.UTF-8
    LC_NUMERIC=C
    LC_TIME=C
    LC_COLLATE=C
    LC_MONETARY=C
    LC_MESSAGES=C
    LC_PAPER=C
    LC_NAME=C
    LC_ADDRESS=C
    LC_TELEPHONE=C
    LC_MEASUREMENT=C
    LC_IDENTIFICATION=C
l4: *
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 436 | C++98 | for overload (7), it was unclear whether `Facet` can be
      cv-qualified | it can be const-qualified, but not volatile-qualified
  LWG 2295 | C++98 | for overload (7), the constructed locale had no name even
      if `f` is null | it has the name of `other`

### See also

- **(destructor)** — destructs the locale and the facets whose reference count
  becomes zero (public member function)

### External links

  1. | List of Windows locale names.
  2. | List of Linux locale names.

---
*Source: https://en.cppreference.com/w/cpp/locale/locale/locale*
