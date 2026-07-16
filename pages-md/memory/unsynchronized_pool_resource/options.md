# std::pmr::unsynchronized_pool_resource::options

```cpp
std::pmr::pool_options options() const;  // (since C++17)
```

Returns the options that controls the pooling behavior of this resource.

The values in the returned struct may differ from those supplied to the
constructor in the following ways:

- Values of zero will be replaced with implementation-specified defaults;
- Sizes may be rounded to an unspecified granularity.

### See also

- **(constructor)** — constructs an `unsynchronized_pool_resource` (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/unsynchronized_pool_resource/options*
