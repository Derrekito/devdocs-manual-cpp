# std::thread::id

```cpp
class thread::id;  // (since C++11)
```

The class `thread::id` is a lightweight, trivially copyable class that serves as
a unique identifier of `std::thread` and `std::jthread`(since C++20) objects.

Instances of this class may also hold the special distinct value that does not
represent any thread. Once a thread has finished, the value of `std::thread::id`
may be reused by another thread.

This class is designed for use as key in associative containers, both ordered
and unordered.

### Member functions

- **(constructor)** — constructs an id that does not represent a thread (public
  member function)

### Non-member functions

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — compares two `thread::id` objects
  (function)
- **operator<<** — serializes a `thread::id` object (function template)

### Helper classes

- **std::hash<std::thread::id> (C++11)** — hash support for
  **`std::thread::id`** (class template specialization)
- **std::formatter<std::thread::id> (C++23)** — formatting support for
  `thread::id` (class template specialization)

### See also

- **get_id** — returns the *id* of the thread (public member function)
- **get_id (C++11)** — returns the thread id of the current thread (function)

---
*Source: https://en.cppreference.com/w/cpp/thread/thread/id*
