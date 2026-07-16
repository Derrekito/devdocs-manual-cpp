# std::hash<std::bitset>

```cpp
template< std::size_t N >
struct hash<std::bitset<N>>;  // (since C++11)
```

The template specialization of `std::hash` for `std::bitset<N>` allows users to
obtain hashes of objects of type `std::bitset<N>`.

### Example

The following code shows one possible output of a hash function used on several
bitsets:

```cpp
#include <bitset>
#include <functional>
#include <iostream>

int main()
{
    std::bitset<4> b1{0}, b2{42};
    std::bitset<8> b3{0}, b4{42};

    std::hash<std::bitset<4>> hash_fn4;
    std::hash<std::bitset<8>> hash_fn8;
    using bin64 = std::bitset<64>;

    std::cout << std::hex
              << bin64{hash_fn4(b1)} << " = " << hash_fn4(b1) << '\n'
              << bin64{hash_fn4(b2)} << " = " << hash_fn4(b2) << '\n'
              << bin64{hash_fn8(b3)} << " = " << hash_fn8(b3) << '\n'
              << bin64{hash_fn8(b4)} << " = " << hash_fn8(b4) << '\n';
}
```

Possible output:

```text
0110110100001001111011100010011011010101100001100011011000011001 = 6d09ee26d5863619
1111111101011100010110100000111000111110100000111011100011110000 = ff5c5a0e3e83b8f0
0110110100001001111011100010011011010101100001100011011000011001 = 6d09ee26d5863619
0101110000011100011110011010111011100110010000110100110001001101 = 5c1c79aee6434c4d
```

### See also

- **hash (C++11)** — hash function object (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/bitset/hash*
