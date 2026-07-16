# std::launch

```cpp
enum class launch : /* unspecified */ {
    async =    /* unspecified */,
    deferred = /* unspecified */,
    /* implementation-defined */
};  // (since C++11)
```

Specifies the launch policy for a task executed by the `std::async` function.
`std::launch` is an enumeration used as BitmaskType.

The following constants denoting individual bits are defined by the standard
library:

- **`std::launch::async`** — the task is executed on a different thread,
  potentially by creating and launching it first
- **`std::launch::deferred`** — the task is executed on the calling thread the
  first time its result is requested (lazy evaluation)

In addition, implementations are allowed to:

- define additional bits and bitmasks to specify restrictions on task
  interactions applicable to a subset of launch policies, and
- enable those additional bitmasks for the first (default) overload of
  `std::async`.

### See also

- **async (C++11)** — runs a function asynchronously (potentially in a new
  thread) and returns a `std::future` that will hold the result (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/thread/launch*
