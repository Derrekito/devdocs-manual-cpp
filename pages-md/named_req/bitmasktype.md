# C++ named requirements: BitmaskType

Defines a type that can be used to represent a set of constant values or any
combination of those values. This trait is typically implemented by integer
types, `std::bitset`, or enumerations (scoped and unscoped) with additional
operator overloads.

### Requirements

The bitmask type supports a finite number of bitmask elements, which are
distinct non-zero values of the bitmask type, such that, for any pair `Ci` and
`Cj`, `Ci & Ci` is nonzero and `Ci & Cj` is zero. In addition, the value `​0​`
is used to represent an empty bitmask, with no values set.

The bitwise operators `operator&`, `operator|`, `operator^`, `operator~`,
`operator&=`, `operator|=`, and `operator^=` are defined for values of the
bitmask type and have the same semantics as the corresponding built-in operators
on unsigned integers would have if the bitmask elements were the distinct
integer powers of two.

The following expressions are well-formed and have the following meaning for any
BitmaskType:

- **`X |= Y`** — sets the value `Y` in the object `X`
- **`X &= ~Y`** — clears the value `Y` in the object `X`
- **`X & Y`** — nonzero result indicates that the value `Y` is set in the object
  `X`

Each representable bitmask element is defined as a `inline`(since
C++17)`constexpr` value of the bitmask type.

### Usage

The following standard library types satisfy BitmaskType:

- `std::chars_format` (since C++17)
- `std::ctype_base::mask`
- `std::ios_base::fmtflags`
- `std::ios_base::iostate`
- `std::ios_base::openmode`
- `std::regex_traits::char_class_type` (since C++11)
- `std::regex_constants::syntax_option_type` (since C++11)
- `std::regex_constants::match_flag_type` (since C++11)
- `std::launch` (since C++11)
- `std::filesystem::perms` (since C++17)
- `std::filesystem::perm_options` (since C++17)
- `std::filesystem::copy_options` (since C++17)
- `std::filesystem::directory_options` (since C++17)

Code that relies on some particular implementation option (e.g. `int n =
std::ios_base::hex`), is nonportable because `std::ios_base::fmtflags` is not
necessarily implicitly convertible to `int`.

---
*Source: https://en.cppreference.com/w/cpp/named_req/BitmaskType*
