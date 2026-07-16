# std::chrono::tzdb_list

```cpp
class tzdb_list;  // (since C++20)
```

`tzdb_list` is a singleton list of `std::chrono::tzdb`s, each of which
represents a copy of the IANA time zone database. Users cannot construct a
`tzdb_list` and can only obtain access to one via the
`std::chrono::get_tzdb_list` free function.

### Member types

- **`const_iterator`** — constant LegacyForwardIterator whose value type is
  `std::chrono::tzdb`

### Member functions

- **(constructor) (deleted)** — copy constructor is defined as deleted (public
  member function)
- **operator= (deleted)** — copy assignment operator is defined as deleted
  (public member function)
- **front** — access the first element (public member function)
- **erase_after** — erases an element after an element (public member function)
- **begincbegin** — returns an iterator to the beginning of the list (public
  member function)
- **endcend** — returns an iterator past the end of the list (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/tzdb_list*
