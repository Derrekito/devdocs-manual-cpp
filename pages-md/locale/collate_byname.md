# std::collate_byname

```cpp
template< class CharT >
class collate_byname : public std::collate<CharT>;
```

`std::collate_byname` is a `std::collate` facet which encapsulates
locale-specific collation (comparison) and hashing of strings. Just like
`std::collate`, it can be imbued in `std::regex` and applied, by means of
`std::locale::operator()`, directly to all standard algorithms that expect a
string comparison predicate.

### Specializations

The standard library is guaranteed to provide the following specializations:

- **std::collate_byname<char>** — locale-specific collation of multibyte strings
- **std::collate_byname<wchar_t>** — locale-specific collation of wide strings

### Member functions

- ****(constructor)**** — constructs a new `collate_byname` facet (public member
  function)
- ****(destructor)**** — destroys a `collate_byname` facet (protected member
  function)

## std::collate_byname::collate_byname

```cpp
explicit collate_byname( const char* name, std::size_t refs = 0 );
explicit collate_byname( const std::string& name, std::size_t refs = 0 );  // (since C++11)
```

Constructs a new `std::collate_byname` facet for a locale with `name`.

`refs` is used for resource management: if `refs == 0`, the implementation
destroys the facet, when the last `std::locale` object holding it is destroyed.
Otherwise, the object is not destroyed.

### Parameters

- **name** — the name of the locale
- **refs** — the number of references that link to the facet

## std::collate_byname::~collate_byname

```cpp
protected:
~collate_byname();
```

Destroys the facet.

## Inherited from std::collate

### Member types

- **`char_type`** — `CharT`
- **`string_type`** — std::basic_string<CharT>

### Member functions

- **compare** — invokes `do_compare` (public member function of
  `std::collate<CharT>`)
- **transform** — invokes `do_transform` (public member function of
  `std::collate<CharT>`)
- **hash** — invokes `do_hash` (public member function of `std::collate<CharT>`)

### Protected member functions

- **do_compare [virtual]** — compares two strings using this facet's collation
  rules (virtual protected member function of `std::collate<CharT>`)
- **do_transform [virtual]** — transforms a string so that collation can be
  replaced by comparison (virtual protected member function of
  `std::collate<CharT>`)
- **do_hash [virtual]** — generates an integer hash value using this facet's
  collation rules (virtual protected member function of `std::collate<CharT>`)

### Notes

Collation order is the dictionary order: the position of the letter in the
national alphabet (its *equivalence class*) has higher priority than its case or
variant. Within an equivalence class, lowercase characters collate before their
uppercase equivalents and locale-specific order may apply to the characters with
diacritics. In some locales, groups of characters compare as single *collation
units*. For example, `"ch"` in Czech follows `"h"` and precedes `"i"`, and
`"dzs"` in Hungarian follows `"dz"` and precedes `"g"`.

### Example

### See also

- **collate** — defines lexicographical comparison and hashing of strings (class
  template)
- **strcoll** — compares two strings in accordance to the current locale
  (function)
- **wcscoll** — compares two wide strings in accordance to the current locale
  (function)
- **operator()** — lexicographically compares two strings using this locale's
  collate facet (public member function of `std::locale`)

---
*Source: https://en.cppreference.com/w/cpp/locale/collate_byname*
