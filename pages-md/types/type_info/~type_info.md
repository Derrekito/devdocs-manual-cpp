# std::type_info::~type_info

```cpp
virtual ~type_info();
```

Destructs an object of type `std::type_info`. This destructor is public virtual,
making the `std::type_info` a polymorphic class.

### Notes

The dynamic type of a `std::type_info` subobject may be examined by the `typeid`
operator.

It is unspecified whether the implementation calls this destructor for any
`std::type_info` object at the end of the program.

---
*Source: https://en.cppreference.com/w/cpp/types/type_info/%7Etype_info*
