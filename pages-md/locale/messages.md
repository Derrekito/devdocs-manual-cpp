# std::messages

```cpp
template<
    class CharT
> class messages;
```

Class template `std::messages` is a standard locale facet that encapsulates
retrieval of strings from message catalogs, such as the ones provided by GNU
gettext or by POSIX `catgets`.

The source of the messages is implementation-defined.

### Specializations

The standard library is guaranteed to provide the following specializations
(they are required to be implemented by any locale object):

- **std::messages<char>** — accesses narrow string message catalog
- **std::messages<wchar_t>** — accesses wide string message catalog

### Member types

- **`char_type`** — `CharT`
- **`string_type`** — std::basic_string<CharT>

### Member functions

- **(constructor)** — constructs a new `messages` facet (public member function)
- **(destructor)** — destructs a `messages` facet (protected member function)
- **open** — invokes `do_open` (public member function)
- **get** — invokes `do_get` (public member function)
- **close** — invokes `do_close` (public member function)

### Member objects

- **static std::locale::id id** — *id* of the locale (public member object)

### Protected member functions

- **do_open [virtual]** — opens a named message catalog (virtual protected
  member function)
- **do_get [virtual]** — retrieves a message from an open message catalog
  (virtual protected member function)
- **do_close [virtual]** — closes a message catalog (virtual protected member
  function)

## Inherited from std::messages_base

- **`catalog`** — /* unspecified signed integer type */

### See also

- **messages_base** — defines messages catalog type (class)
- **messages_byname** — represents the system-supplied `std::messages` for the
  named locale (class template)

---
*Source: https://en.cppreference.com/w/cpp/locale/messages*
