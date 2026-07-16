# std::time_get_byname

```cpp
template<
    class CharT,
    class InputIt = std::istreambuf_iterator<CharT>
> class time_get_byname : public std::time_get<CharT, InputIt>
```

`std::time_get_byname` is a `std::time_get` facet which encapsulates time and
date parsing rules of the locale specified at its construction.

### Specializations

The standard library is guaranteed to provide every specialization that
satisfies the following type requirements:

- `CharT` is one of char and wchar_t, and
- `InputIt` must meet the requirements of LegacyInputIterator.

### Member types

- **`char_type`** — `CharT`
- **`iter_type`** — `InputIt`

### Member functions

- ****(constructor)**** — constructs a new `time_get_byname` facet (public
  member function)
- ****(destructor)**** — destroys a `time_get_byname` facet (protected member
  function)

## std::time_get_byname::time_get_byname

```cpp
explicit time_get_byname( const char* name, std::size_t refs = 0 );
explicit time_get_byname( const std::string& name, std::size_t refs = 0 );  // (since C++11)
```

Constructs a new `std::time_get_byname` facet for a locale with `name`.

`refs` is used for resource management: if `refs == 0`, the implementation
destroys the facet, when the last `std::locale` object holding it is destroyed.
Otherwise, the object is not destroyed.

### Parameters

- **name** — the name of the locale
- **refs** — the number of references that link to the facet

## std::time_get_byname::~time_get_byname

```cpp
protected:
~time_get_byname();
```

Destroys the facet.

## Inherited from std::time_get

- **`char_type`** — `CharT`
- **`iter_type`** — `InputIt`

### Member objects

- **`id` (static)** — `std::locale::id`

### Member functions

- **date_order** — invokes `do_date_order` (public member function of
  `std::time_get<CharT,InputIt>`)
- **get_time** — invokes `do_get_time` (public member function of
  `std::time_get<CharT,InputIt>`)
- **get_date** — invokes `do_get_date` (public member function of
  `std::time_get<CharT,InputIt>`)
- **get_weekday** — invokes `do_get_weekday` (public member function of
  `std::time_get<CharT,InputIt>`)
- **get_monthname** — invokes `do_get_monthname` (public member function of
  `std::time_get<CharT,InputIt>`)
- **get_year** — invokes `do_get_year` (public member function of
  `std::time_get<CharT,InputIt>`)
- **get (C++11)** — invokes `do_get` (public member function of
  `std::time_get<CharT,InputIt>`)

### Protected member functions

- **do_date_order [virtual]** — obtains preferred ordering of day, month, and
  year (virtual protected member function of `std::time_get<CharT,InputIt>`)
- **do_get_time [virtual]** — extracts hours, minutes, and seconds from input
  stream (virtual protected member function of `std::time_get<CharT,InputIt>`)
- **do_get_date [virtual]** — extracts month, day, and year from input stream
  (virtual protected member function of `std::time_get<CharT,InputIt>`)
- **do_get_weekday [virtual]** — extracts the name of a day of the week from
  input stream (virtual protected member function of
  `std::time_get<CharT,InputIt>`)
- **do_get_monthname [virtual]** — extracts a month name from input stream
  (virtual protected member function of `std::time_get<CharT,InputIt>`)
- **do_get_year [virtual]** — extracts a year from input stream (virtual
  protected member function of `std::time_get<CharT,InputIt>`)
- **do_get [virtual] (C++11)** — extracts date/time components from input
  stream, according to the specified format (virtual protected member function
  of `std::time_get<CharT,InputIt>`)

## Inherited from std::time_base

- **`dateorder`** — date order enumeration type, defining the values `no_order`,
  `dmy`, `mdy`, `ymd`, and `ydm`

### Example

### See also

- **time_get** — parses time/date values from an input character sequence into
  `std::tm` (class template)
- **get_time (C++11)** — parses a date/time value of specified format (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/locale/time_get_byname*
