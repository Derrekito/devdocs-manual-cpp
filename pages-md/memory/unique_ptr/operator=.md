# std::unique_ptr<T,Deleter>::operator=

```cpp
unique_ptr& operator=( unique_ptr&& r ) noexcept;  // (1) (constexpr since C++23)
template< class U, class E >
unique_ptr& operator=( unique_ptr<U, E>&& r ) noexcept;  // (2) (constexpr since C++23)
unique_ptr& operator=( std::nullptr_t ) noexcept;  // (3) (constexpr since C++23)
unique_ptr& operator=( const unique_ptr& ) = delete;  // (4)
```

1) Move assignment operator. Transfers ownership from `r` to `*this` as if by
   calling `reset(r.release())` followed by assigning `get_deleter()` from
   `std::forward<Deleter>(r.get_deleter())`.

This overload participates in overload resolution only if
   `std::is_move_assignable<Deleter>::value` is `true`.
If `Deleter` is not a reference type, the behavior is undefined if

- `Deleter` is not MoveAssignable, or
- assigning `get_deleter()` from an rvalue of type `Deleter` would throw an
  exception.

Otherwise (`Deleter` is a reference type), the behavior is undefined if

- `std::remove_reference<Deleter>::type` is not CopyAssignable, or
- assigning `get_deleter()` from an lvalue of type `Deleter` would throw an
  exception.

2) Converting assignment operator. Transfers ownership from `r` to `*this` as if
   by calling `reset(r.release())` followed by assigning `get_deleter()` from
   `std::forward<E>(r.get_deleter())`.
For the primary template, this overload participates in overload resolution only
if

- `U` is not an array type,
- `unique_ptr<U, E>::pointer` is implicitly convertible to `pointer`, and
- `std::is_assignable<Deleter&, E&&>::value` is `true`.

For the array specialization (`unique_ptr<T[]>`), this overload participates in
overload resolution only if

- `U` is an array type,
- `pointer` is the same type as `element_type*`,
- `unique_ptr<U, E>::pointer` is the same type as `unique_ptr<U,
  E>::element_type*`,
- `unique_ptr<U, E>::element_type(*)[]` is convertible to `element_type(*)[]`,
  and
- `std::is_assignable<Deleter&, E&&>::value` is `true`.

If `E` is not a reference type, the behavior is undefined if assigning
   `get_deleter()` from an rvalue of type `E` is ill-formed or would throw an
   exception.

Otherwise (`E` is a reference type), the behavior is undefined if assigning
   `get_deleter()` from an lvalue of type `E` is ill-formed or would throw an
   exception.

3) Effectively the same as calling `reset()`.

4) Copy assignment operator is explicitly deleted.

### Parameters

- **r** — smart pointer from which ownership will be transferred

### Return value

`*this`

### Notes

As a move-only type, `unique_ptr`'s assignment operator only accepts rvalues
arguments (e.g. the result of `std::make_unique` or a `std::move`'d `unique_ptr`
variable).

### Example

```cpp
#include <iostream>
#include <memory>

struct Foo
{
    int id;
    Foo(int id) : id(id) { std::cout << "Foo " << id << '\n'; }
    ~Foo() { std::cout << "~Foo " << id << '\n'; }
};

int main()
{
    std::unique_ptr<Foo> p1(std::make_unique<Foo>(1));

    {
        std::cout << "Creating new Foo...\n";
        std::unique_ptr<Foo> p2(std::make_unique<Foo>(2));
        // p1 = p2; // Error ! can't copy unique_ptr
        p1 = std::move(p2);
        std::cout << "About to leave inner block...\n";

        // Foo instance will continue to live,
        // despite p2 going out of scope
    }

    std::cout << "About to leave program...\n";
}
```

Output:

```text
Foo 1
Creating new Foo...
Foo 2
~Foo 1
About to leave inner block...
About to leave program...
~Foo 2
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2047 | C++11 | for overload (2), `get_deleter()` was assigned from
      `std::forward<Deleter>(r.get_deleter())` | corrected to
      `std::forward<E>(r.get_deleter())`
  LWG 2118 | C++11 | `unique_ptr<T[]>::operator=` rejected qualification
      conversions | accepts
  LWG 2228 | C++11 | the converting assignment operator was not constrained |
      constrained
  LWG 2899 | C++11 | the move assignment operator was not constrained |
      constrained

---
*Source: https://en.cppreference.com/w/cpp/memory/unique_ptr/operator%3D*
