# std::locale

```cpp
class locale;
```

An object of class `std::locale` is an immutable indexed set of immutable
facets. Each stream object of the C++ input/output library is associated with an
`std::locale` object and uses its facets for parsing and formatting of all data.
In addition, a locale object is associated with each `std::basic_regex`
object.(since C++11) Locale objects can also be used as predicates that perform
string collation with the standard containers and algorithms and can be accessed
directly to obtain or modify the facets they hold.

Each locale constructed in a C++ program holds at least the following standard
facets (i.e. `std::has_facet` returns `true` for all these facet types), but a
program may define additional specializations or completely new facets and add
them to any existing locale object.

  Supported facets
  std::collate<char> std::collate<wchar_t> | std::codecvt<char, char,
      std::mbstate_t> std::codecvt<wchar_t, char, std::mbstate_t>
      std::codecvt<char16_t, char8_t, std::mbstate_t> (since C++20)
      std::codecvt<char32_t, char8_t, std::mbstate_t> (since C++20)
  std::ctype<char> std::ctype<wchar_t>
  std::money_get<char> std::money_get<wchar_t> | std::moneypunct<char>
      std::moneypunct<char, true> std::moneypunct<wchar_t>
      std::moneypunct<wchar_t, true>
  std::money_put<char> std::money_put<wchar_t>
  std::num_get<char> std::num_get<wchar_t> | std::numpunct<char>
      std::numpunct<wchar_t>
  std::num_put<char> std::num_put<wchar_t>
  std::time_get<char> std::time_get<wchar_t> | std::messages<char>
      std::messages<wchar_t>
  std::time_put<char> std::time_put<wchar_t>
  Deprecated facets
  std::codecvt<char16_t, char, std::mbstate_t> (since C++11)(deprecated in
      C++20) std::codecvt<char32_t, char, std::mbstate_t> (since
      C++11)(deprecated in C++20)

Internally, a locale object is implemented as if it is a reference-counted
pointer to an array (indexed by `std::locale::id`) of reference-counted pointers
to facets: copying a locale only copies one pointer and increments several
reference counts. To maintain the standard C++ library thread safety guarantees
(operations on different objects are always thread-safe), both the locale
reference count and each facet reference count are updated in a thread-safe
manner, similar to `std::shared_ptr`.

### Member types

- **id** — the facet index type: each facet class must declare or inherit a
  public static member of this type (class)
- **facet** — the base class for all facet categories: each facet of any
  category is derived from this type (class)
- **category** — int (typedef)

### Member objects

- **none [static]** — a zero value of type `category` indicating no facet
  category (public static member constant)
- **collate [static]** — a bitmask value of type `category` indicating the
  collate facet category (public static member constant)
- **ctype [static]** — a bitmask value of type `category` indicating the ctype
  facet category (public static member constant)
- **monetary [static]** — a bitmask value of type `category` indicating the
  monetary facet category (public static member constant)
- **numeric [static]** — a bitmask value of type `category` indicating the
  numeric facet category (public static member constant)
- **time [static]** — a bitmask value of type `category` indicating the time
  facet category (public static member constant)
- **messages [static]** — a bitmask value of type `category` indicating the
  messages facet category (public static member constant)
- **all [static]** — `collate | ctype | monetary | numeric | time | messages`
  (public static member constant)

`std::locale` member functions expecting a `category` argument require one of
the category values defined above, or the union of two or more such values. The
`LC` constants are not accepted.

### Member functions

- **(constructor)** — constructs a new locale (public member function)
- **(destructor)** — destructs the locale and the facets whose reference count
  becomes zero (public member function)
- **operator=** — replaces a locale (public member function)
- **combine** — constructs a locale with compile-time identified facet copied
  from another locale (public member function)
- **name** — returns the name of the locale or "*" if unnamed (public member
  function)
- **operator==operator!= (removed in C++20)** — equality comparison between
  locale objects (public member function)
- **operator()** — lexicographically compares two strings using this locale's
  collate facet (public member function)
- **global [static]** — changes the global locale (public static member
  function)
- **classic [static]** — obtains a reference to the "C" locale (public static
  member function)

### Example

Demonstrates the typical prologue of a locale-sensitive program
(cross-platform).

```cpp
#include <iostream>
#include <locale>

int main()
{
    std::wcout << "User-preferred locale setting is "
               << std::locale("").name().c_str() << '\n';
    // on startup, the global locale is the "C" locale
    std::wcout << 1000.01 << '\n';

    // replace the C++ global locale and the "C" locale with the user-preferred locale
    std::locale::global(std::locale(""));
    // use the new global locale for future wide character output
    std::wcout.imbue(std::locale());

    // output the same number again
    std::wcout << 1000.01 << '\n';
}
```

Possible output:

```text
User-preferred locale setting is en_US.UTF8
1000.01
1,000.01
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 340 | C++98 | the set of standard facets that all locales need to hold was
      unclear | made clear
  LWG 347 | C++98 | parameters of type `category` could accept `LC` constants |
      not accepted anymore

### See also

- **use_facet** — obtains a facet from a locale (function template)
- **has_facet** — checks if a locale implements a specific facet (function
  template)
- **imbue** — sets locale (public member function of `std::ios_base`)
- **getloc** — returns current locale (public member function of
  `std::ios_base`)

---
*Source: https://en.cppreference.com/w/cpp/locale/locale*
