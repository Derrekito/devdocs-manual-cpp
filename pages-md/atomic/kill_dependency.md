# std::kill_dependency

```cpp
template< class T >
T kill_dependency( T y ) noexcept;  // (since C++11)
```

Informs the compiler that the dependency tree started by an
`std::memory_order_consume` atomic load operation does not extend past the
return value of `std::kill_dependency`; that is, the argument does not carry a
dependency into the return value.

This may be used to avoid unnecessary `std::memory_order_acquire` fences when
the dependency chain leaves function scope (and the function does not have the
`[[carries_dependency]]` attribute).

### Parameters

- **y** — the expression whose return value is to be removed from a dependency
  tree

### Return value

Returns `y`, no longer a part of a dependency tree.

### Examples

##### file1.cpp:

```cpp
struct Foo
{
    int* a;
    int* b;
};

std::atomic<Foo*> foo_head[10];
int foo_array[10][10];

// consume operation starts a dependency chain, which escapes this function
[[carries_dependency]] Foo* f(int i)
{
    return foo_head[i].load(memory_order_consume);
}

// the dependency chain enters this function through the right parameter and is
// killed before the function ends (so no extra acquire operation takes place)
int g(int* x, int* y [[carries_dependency]])
{
    return std::kill_dependency(foo_array[*x][*y]);
}
```

##### file2.cpp:

```cpp
[[carries_dependency]] struct Foo* f(int i);
int g(int* x, int* y [[carries_dependency]]);

int c = 3;
void h(int i)
{
    Foo* p;
    p = f(i); // dependency chain started inside f continues into p without undue acquire
    do_something_with(g(&c, p->a)); // p->b is not brought in from the cache
    do_something_with(g(p->a, &c)); // left argument does not have the carries_dependency
                                    // attribute: memory acquire fence may be issued
                                    // p->b becomes visible before g() is entered
}
```

### See also

- **memory_order (C++11)** — defines memory ordering constraints for the given
  atomic operation (enum)

**C documentation for `kill_dependency`**

---
*Source: https://en.cppreference.com/w/cpp/atomic/kill_dependency*
