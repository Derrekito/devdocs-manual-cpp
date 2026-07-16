# delete expression

Destroys object(s) previously allocated by the new-expression and releases
obtained memory area.

### Syntax

```text
::(optional) delete expression    (1)
::(optional) delete[] expression    (2)
```

- **expression** — one of the following: an expression of class type
  contextually implicitly convertible to a pointer to object type a prvalue of
  pointer to object type

1) Destroys one non-array object created by a new-expression.

2) Destroys an array created by a new[]-expression.

### Explanation

Given the pointer evaluated from expression (after possible conversions) as
`ptr`.

1) `ptr` must be one of

- a null pointer,
- a pointer to a non-array object created by a new-expression, or
- a pointer to a base subobject of a non-array object created by a
  new-expression.

The pointed-to type of `ptr` must be similar to the type of the object (or of a
   base subobject). If `ptr` is anything else, including if it is a pointer
   obtained by the array form of new-expression, the behavior is undefined.

2) `ptr` must be a null pointer or a pointer whose value is previously obtained
   by an array form of new-expression whose allocation function was not a
   non-allocating form (i.e. overload (10)).

The pointed-to type of `ptr` must be similar to the element type of the array
   object. If `ptr` is anything else, including if it is a pointer obtained by
   the non-array form of new-expression, the behavior is undefined.

The result of the delete-expression always has type void.

If the object being deleted has incomplete class type at the point of deletion,
and the complete class has a non-trivial destructor or a deallocation function,
the behavior is undefined.

If `ptr` is not a null pointer and the deallocation function is not a destroying
delete(since C++20), the delete-expression invokes the destructor (if any) for
the object that's being destroyed, or for every element of the array being
destroyed (proceeding from the last element to the first element of the array).

After that, whether or not an exception was thrown by any destructor, the
delete-expression invokes the deallocation function: either `operator delete`
(first version) or `operator delete[]` (second version), unless the matching
new-expression was combined with another new-expression(since C++14).

The deallocation function's name is looked up in the scope of the dynamic type
of the object pointed to by `ptr`, which means class-specific deallocation
functions, if present, are found before the global ones. If `::` is present in
the delete-expression, only the global namespace is examined by this lookup. In
any case, any declarations other than of usual deallocation functions are
discarded.

If lookup finds more than one deallocation function, the function to be called
is selected as follows (see deallocation function for a more detailed
description of these functions and their effects):

- If at least one of the deallocation functions is a destroying delete, all
  non-destroying deletes are ignored.
*(since C++20)*

- If the type's alignment requirement exceeds
  `__STDCPP_DEFAULT_NEW_ALIGNMENT__`, alignment-aware deallocation functions
  (with a parameter of type `std::align_val_t`) are preferred. For other types,
  the alignment-unaware deallocation functions (without a parameter of type
  `std::align_val_t`) are preferred.
- If only one function is left, that function is selected.
*(since C++17)*

- If the deallocation functions that were found are class-specific, size-unaware
  class-specific deallocation function (without a parameter of type
  `std::size_t`) is preferred over size-aware class-specific deallocation
  function (with a parameter of type `std::size_t`).

- Otherwise, lookup reached global scope, and:
*(since C++14)*

The pointer to the block of storage to be reclaimed is passed to the
deallocation function that was selected by the process above as the first
argument. The size of the block is passed as the optional `std::size_t`
argument. The alignment requirement is passed as the optional `std::align_val_t`
argument.(since C++17)

If `ptr` is a null pointer value, no destructors are called, and the
deallocation function may or may not be called (it's unspecified), but the
default deallocation functions are guaranteed to do nothing when passed a null
pointer.

If `ptr` is a pointer to a base class subobject of the object that was allocated
with new, the destructor of the base class must be virtual, otherwise the
behavior is undefined.

### Notes

A pointer to void cannot be deleted because it is not a pointer to a complete
object type.

Because a pair of brackets following the keyword delete is always interpreted as
the array form of a delete-expression, a lambda-expression with an empty capture
list immediately after delete must be enclosed in parentheses.
```cpp
// delete []{ return new int; }(); // parse error
delete ([]{ return new int; })();  // OK
```
*(since C++11)*

### Keywords

`delete`

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 288 | C++98 | for the first form, the static type of the operand was
      compared with its dynamic type | compare the static type of the object to
      be deleted with its dynamic type
  CWG 353 | C++98 | whether the deallocation function will be invoked if the
      destructor throws an exception was unspecified | always invoked
  CWG 599 | C++98 | the first form could take a null pointer of any type,
      including function pointers | except pointers to object types, all other
      pointer types are rejected
  CWG 1642 | C++98 | expression could be a pointer lvalue | not allowed
  CWG 2474 | C++98 | deleting a pointer to an object of a similar but different
      type resulted in undefined behavior | made well-defined
  CWG 2624 | C++98 | pointers obtained from non-allocating operator new[] could
      be passed to delete[] | prohibited

### See also

- new

---
*Source: https://en.cppreference.com/w/cpp/language/delete*
