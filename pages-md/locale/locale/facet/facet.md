# std::locale::facet::facet

```cpp
explicit facet( std::size_t refs = 0 );  // (1)
facet( const facet& ) = delete;  // (2)
```

1) Creates a facet with starting reference count `refs`. If `refs` is non-zero,
   the facet will not be deleted when the last locale referencing it goes out of
   scope. A facet with static or dynamic storage duration should always be
   constructed with a non-zero `refs`.

2) Copy constructor is deleted; `std::locale::facet` is not copyable.

---
*Source: https://en.cppreference.com/w/cpp/locale/locale/facet/facet*
