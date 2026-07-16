# std::scoped_allocator_adaptor<OuterAlloc,InnerAlloc...>::destroy

```cpp
template< class T >
void destroy( T* p );  // (since C++11)
```

Uses the outer allocator to call the destructor of the object pointed to by `p`,
by calling

`std::allocator_traits<OUTERMOST>::destroy(OUTERMOST(*this), p)`

where OUTERMOST is the type that would be returned by calling
`this->outer_allocator()`, and then calling the `outer_allocator()` member
function recursively on the result of this call until reaching the type that has
no such member function.

### Parameters

- **p** — pointer to the object that is going to be destroyed

### Return value

(none)

### See also

- **destroy [static]** — destructs an object stored in the allocated storage
  (function template)
- **destroy (until C++20)** — destructs an object in allocated storage (public
  member function of `std::allocator<T>`)

---
*Source: https://en.cppreference.com/w/cpp/memory/scoped_allocator_adaptor/destroy*
