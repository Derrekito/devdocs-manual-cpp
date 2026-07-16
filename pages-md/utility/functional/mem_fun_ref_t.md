# std::mem_fun_ref_t, std::mem_fun1_ref_t, std::const_mem_fun_ref_t, std::const_mem_fun1_ref_t

```cpp
template< class S, class T >
class mem_fun_ref_t : public unary_function<T,S> {
public:
    explicit mem_fun_ref_t(S (T::*p)());
    S operator()(T& p) const;
};  // (1) (deprecated in C++11) (removed in C++17)
template< class S, class T >
class const_mem_fun_ref_t : public unary_function<T,S> {
public:
    explicit const_mem_fun_ref_t(S (T::*p)() const);
    S operator()(const T& p) const;
};  // (2) (deprecated in C++11) (removed in C++17)
template< class S, class T, class A >
class mem_fun1_ref_t : public binary_function<T,A,S> {
public:
    explicit mem_fun1_ref_t(S (T::*p)(A));
    S operator()(T& p, A x) const;
};  // (3) (deprecated in C++11) (removed in C++17)
template< class S, class T, class A >
class const_mem_fun1_ref_t : public binary_function<T,A,S> {
public:
    explicit const_mem_fun1_ref_t(S (T::*p)(A) const);
    S operator()(const T& p, A x) const;
};  // (4) (deprecated in C++11) (removed in C++17)
```

Wrapper around a member function pointer. The class instance whose member
function to call is passed as a reference to the `operator()`.

1) Wraps a non-const member function with no parameters.

2) Wraps a const member function with no parameters.

3) Wraps a non-const member function with a single parameter.

4) Wraps a const member function with a single parameter.

### See also

- **mem_fun_ref (deprecated in C++11)(removed in C++17)** — creates a wrapper
  from a pointer to member function, callable with a reference to object
  (function template)
- **mem_fun_tmem_fun1_tconst_mem_fun_tconst_mem_fun1_t (deprecated in
  C++11)(removed in C++17)** — wrapper for a pointer to nullary or unary member
  function, callable with a pointer to object (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/mem_fun_ref_t*
