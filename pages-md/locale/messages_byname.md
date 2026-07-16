# std::messages_byname

```cpp
template< class CharT >
class messages_byname : public std::messages<CharT>;
```

`std::messages_byname` is a `std::messages` facet which encapsulates retrieval
of strings from message catalogs of the locale specified at its construction.

### Specializations

The standard library is guaranteed to provide the following specializations:

- **std::messages_byname<char>** — narrow/multibyte message catalog access
- **std::messages_byname<wchar_t>** — wide string message catalog access

### Member types

- **`catalog`** — std::messages_base<CharT>::catalog
- **`string_type`** — std::basic_string<CharT>

### Member functions

- ****(constructor)**** — constructs a new `messages_byname` facet (public
  member function)
- ****(destructor)**** — destroys a `messages_byname` facet (protected member
  function)

## std::messages_byname::messages_byname

```cpp
explicit messages_byname( const char* name, std::size_t refs = 0 );
explicit messages_byname( const std::string& name, std::size_t refs = 0 );  // (since C++11)
```

Constructs a new `std::messages_byname` facet for a locale with `name`.

`refs` is used for resource management: if `refs == 0`, the implementation
destroys the facet, when the last `std::locale` object holding it is destroyed.
Otherwise, the object is not destroyed.

### Parameters

- **name** — the name of the locale
- **refs** — the number of references that link to the facet

## std::messages_byname::~messages_byname

```cpp
protected:
~messages_byname();
```

Destroys the facet.

## Inherited from std::messages

### Member types

- **`char_type`** — `CharT`
- **`string_type`** — std::basic_string<CharT>

### Member objects

- **`id` (static)** — `std::locale::id`

### Member functions

- **open** — invokes `do_open` (public member function of
  `std::messages<CharT>`)
- **get** — invokes `do_get` (public member function of `std::messages<CharT>`)
- **close** — invokes `do_close` (public member function of
  `std::messages<CharT>`)

### Protected member functions

- **do_open [virtual]** — opens a named message catalog (virtual protected
  member function of `std::messages<CharT>`)
- **do_get [virtual]** — retrieves a message from an open message catalog
  (virtual protected member function of `std::messages<CharT>`)
- **do_close [virtual]** — closes a message catalog (virtual protected member
  function of `std::messages<CharT>`)

### Example

```cpp
#include <iostream>
#include <locale>

void try_with(const std::locale& loc)
{
    const std::messages<char>& facet = std::use_facet<std::messages<char>>(loc);

    std::messages<char>::catalog cat = facet.open("sed", std::cout.getloc());
    if (cat < 0)
        std::cout << "Could not open \"sed\" message catalog\n";
    else
        std::cout << "\"No match\" "
                  << facet.get(cat, 0, 0, "No match") << '\n'
                  << "\"Memory exhausted\" "
                  << facet.get(cat, 0, 0, "Memory exhausted") << '\n';

    facet.close(cat);
}

int main()
{
    std::locale loc("en_US.utf8");
    std::cout.imbue(loc);

    try_with(std::locale(loc, new std::messages_byname<char>("de_DE.utf8")));
    try_with(std::locale(loc, new std::messages_byname<char>("fr_FR.utf8")));
    try_with(std::locale(loc, new std::messages_byname<char>("ja_JP.utf8")));
}
```

Possible output:

```text
"No match" Keine Übereinstimmung
"Memory exhausted" Speicher erschöpft
"No match" Pas de concordance
"Memory exhausted" Mémoire épuisée
"No match" 照合しません
"Memory exhausted" メモリーが足りません
```

### See also

- **messages** — implements retrieval of strings from message catalogs (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/locale/messages_byname*
