# std::type_info::operator==, std::type_info::operator!=

```cpp
bool operator==( const type_info& rhs ) const;  // (until C++11)
bool operator==( const type_info& rhs ) const noexcept;  // (since C++11) (until C++23)
constexpr bool operator==( const type_info& rhs ) const noexcept;  // (since C++23)
bool operator!=( const type_info& rhs ) const;  // (until C++11)
bool operator!=( const type_info& rhs ) const noexcept;  // (since C++11) (until C++20)
```

Checks if the objects refer to the same types.

The `!=` operator is synthesized from `operator==`.
*(since C++20)*

### Parameters

- **rhs** — another type information object to compare to

### Return value

`true` if the comparison operation holds true, `false` otherwise.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_constexpr_typeinfo` | 202106L | (C++23) | Constexpr for
      `std::type_info::operator==`

### Example

```cpp
#include <iostream>
#include <string>
#include <typeinfo>
#include <utility>

class person
{
public:
    explicit person(std::string n) : name_(std::move(n)) {}
    virtual const std::string& name() const { return name_; }

private:
    std::string name_;
};

class employee : public person
{
public:
    employee(std::string n, std::string p)
        : person(std::move(n)), profession_(std::move(p)) {}

    const std::string& profession() const { return profession_; }

private:
    std::string profession_;
};

void print_info(const person& p)
{
    if (typeid(person) == typeid(p))
        std::cout << p.name() << " is not an employee\n";
    else if (typeid(employee) == typeid(p))
    {
        std::cout << p.name() << " is an employee ";
        auto& emp = dynamic_cast<const employee&>(p);
        std::cout << "who works in " << emp.profession() << '\n';
    }
}

int main()
{
    print_info(employee{"Paul","Economics"});
    print_info(person{"Kate"});

#if __cpp_lib_constexpr_typeinfo
    if constexpr (typeid(employee) != typeid(person)) // C++23
        std::cout << "class `employee` != class `person`\n";
#endif
}
```

Possible output:

```text
Paul is an employee who works in Economics
Kate is not an employee
class `employee` != class `person`
```

### See also

- **before** — checks whether the referred type precedes referred type of
  another `type_info` object in the implementation defined order, i.e. orders
  the referred types (public member function)

---
*Source: https://en.cppreference.com/w/cpp/types/type_info/operator_cmp*
