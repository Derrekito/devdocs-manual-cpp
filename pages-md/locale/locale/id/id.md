# std::locale::id::id

```cpp
id();  // (1)
id( const id& ) = delete;  // (2)
```

1) Default constructor: creates an object of type `std::locale::id` with
   implementation-specific content.

2) Copy constructor is deleted; `std::locale::id` is not copyable.

### Notes

Because locales and facets must be available for the IO stream objects with
static storage duration, such as `std::cout`, typical implementations let
implicit default constructor zero-initialize the contents of `std::locale::id`
during static initialization (before constructors run for static objects), and
when a facet is added to any locale for the first time, the locale completes
initialization of the facet's `id`.

---
*Source: https://en.cppreference.com/w/cpp/locale/locale/id/id*
