# std::type_info

```cpp
class type_info;
```

The class `type_info` holds implementation-specific information about a type,
including the name of the type and means to compare two types for equality or
collating order. This is the class returned by the `typeid` operator.

The `type_info` class is neither CopyConstructible nor CopyAssignable.

### Member functions

- **(constructor) [deleted]** — has neither default nor copy constructors
  (public member function)
- **(destructor) [virtual]** — the virtual destructor makes `type_info` a
  polymorphic class (virtual public member function)
- **operator= [deleted]** — can not be copy-assigned (public member function)
- **operator==operator!= (removed in C++20)** — checks whether the objects refer
  to the same type (public member function)
- **before** — checks whether the referred type precedes referred type of
  another `type_info` object in the implementation defined order, i.e. orders
  the referred types (public member function)
- **hash_code (C++11)** — returns a value which is identical for the same types
  (public member function)
- **name** — implementation defined name of the type (public member function)

### See also

- **type_index (C++11)** — wrapper around a `type_info` object, that can be used
  as index in associative and unordered associative containers (class)
- **`typeid`** — Queries information of a type, returning a `std::type_info`
  object representing the type (built-in operator)

---
*Source: https://en.cppreference.com/w/cpp/types/type_info*
