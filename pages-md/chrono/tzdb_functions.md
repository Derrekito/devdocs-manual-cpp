# std::chrono::get_tzdb_list, std::chrono::get_tzdb, std::chrono::remote_version, std::chrono::reload_tzdb

```cpp
std::chrono::tzdb_list& get_tzdb_list();  // (1) (since C++20)
const std::chrono::tzdb& get_tzdb();  // (2) (since C++20)
std::string remote_version();  // (3) (since C++20)
const std::chrono::tzdb& reload_tzdb();  // (4) (since C++20)
```

These functions provide access to the program-wide time zone database.

1) Returns a reference to the global `std::chrono::tzdb_list` singleton. If this
   is the first access to the database, initialize the database. After the
   initialization, the database will hold a single initialized
   `std::chrono::tzdb` object. This function is thread-safe: concurrent calls to
   this function from multiple threads do not introduce a data race.

2) Returns a reference to the first `std::chrono::tzdb` object held by the
   `tzdb_list` singleton. Equivalent to `std::chrono::get_tzdb_list().front()`.

3) Returns a string containing the latest remote database version.

4) If `remote_version() != get_tzdb().version`, pushes a new `tzdb` object
   representing the remote database to the front of the `tzdb_list` singleton
   referenced by `get_tzdb_list()`. Otherwise there are no effects. No
   references, pointers or iterators are invalidated. Calling this function
   concurrently with `get_tzdb_list().front()` or
   `get_tzdb_list().erase_after()` does not introduce a data race.

### Exceptions

1) `std::runtime_error` if for any reason a reference to a `tzdb_list`
   containing one or more valid `tzdb` cannot be returned.

### Return value

1) A reference to the global `std::chrono::tzdb_list` singleton.

2) `std::chrono::get_tzdb_list().front()`.

3) A string containing the latest remote database version.

4) `std::chrono::get_tzdb_list().front()` (after any update made by this
   function).

---
*Source: https://en.cppreference.com/w/cpp/chrono/tzdb_functions*
