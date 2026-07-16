# std::shared_ptr<T>::operator=

```cpp
shared_ptr& operator=( const shared_ptr& r ) noexcept;  // (1)
template< class Y >
shared_ptr& operator=( const shared_ptr<Y>& r ) noexcept;  // (2)
shared_ptr& operator=( shared_ptr&& r ) noexcept;  // (3)
template< class Y >
shared_ptr& operator=( shared_ptr<Y>&& r ) noexcept;  // (4)
template< class Y >
shared_ptr& operator=( std::auto_ptr<Y>&& r );  // (5) (deprecated in C++11) (removed in C++17)
template< class Y, class Deleter >
shared_ptr& operator=( std::unique_ptr<Y, Deleter>&& r );  // (6)
```

Replaces the managed object with the one managed by `r`.

If `*this` already owns an object and it is the last `shared_ptr` owning it, and
`r` is not the same as `*this`, the object is destroyed through the owned
deleter.

1,2) Shares ownership of the object managed by `r`. If `r` manages no object,
   `*this` manages no object too. Equivalent to `shared_ptr<T>(r).swap(*this)`.

3,4) Move-assigns a `shared_ptr` from `r`. After the assignment, `*this`
   contains a copy of the previous state of `r`, and `r` is empty. Equivalent to
   `shared_ptr<T>(std::move(r)).swap(*this)`.

5) Transfers the ownership of the object managed by `r` to `*this`. If `r`
   manages no object, `*this` manages no object too. After the assignment,
   `*this` contains the pointer previously held by `r`, and `use_count() == 1`;
   also `r` is empty. Equivalent to `shared_ptr<T>(r).swap(*this)`.

6) Transfers the ownership of the object managed by `r` to `*this`. The deleter
   associated to `r` is stored for future deletion of the managed object. `r`
   manages no object after the call. Equivalent to
   `shared_ptr<T>(std::move(r)).swap(*this)`.

### Parameters

- **r** — another smart pointer to share the ownership to or acquire the
  ownership from

### Return value

`*this`

### Notes

The implementation may meet the requirements without creating a temporary
`shared_ptr` object.

### Exceptions

5,6) May throw implementation-defined exceptions.

### Example

### See also

- **reset** — replaces the managed object (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/shared_ptr/operator%3D*
